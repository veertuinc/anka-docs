---
date: 2026-08-25T00:00:00-00:00
title: "macOS CI for a physical-goods enterprise"
linkTitle: "Physical-goods enterprise"
weight: 5
description: >
  EC2 Mac Marketplace AMI with Controller and Registry CI for retail iOS apps and simulator tests in Anka VMs
---

## Context

A large company that makes physical products. It also ships iOS apps for stores, factories, and consumers. Several brands or regions run their own mobile pipelines on a shared Mac fleet. Central IT operates [Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/_index.md" >}}) entirely in AWS: a [Controller and Registry]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) on a Linux EC2 instance and [EC2 Mac]({{< relref "aws-ec2-mac/_index.md" >}}) nodes from the [Marketplace AMI]({{< relref "aws-ec2-mac/marketplace-ami.md" >}}), all in the same region and VPC.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Brand CI on central EC2 Mac fleet]({{< siteurl >}}images/use-cases/physical-goods-enterprise/build-ecosystem.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Problem

Each brand bought a few Mac minis and ran builds on the host. Images diverged. Simulator state leaked between jobs. One region had no spare host for a store submission. Another region had idle hardware. Central IT could not say how many Macs existed or which Xcode build a release used.

Jobs on those hosts also let one brand reach another brand’s source or the node itself. A VM boundary is required when brands share hardware.

They needed:

- One image pipeline that brands can still customize.
- A Mac fleet that can grow for a store freeze without buying hardware per region.
- No annual Anka license renewal or fulfillment tracking on each node.
- iOS simulator tests inside the CI VM, not on the bare-metal host.
- Support for more than one CI product without a second virtualization stack.
- Isolation so one brand’s VM cannot reach another brand’s VM or the Mac node.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Bare metal host risks versus isolated VMs]({{< siteurl >}}images/use-cases/physical-goods-enterprise/bare-metal-risk.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Deployment model

All Mac CI runs on [AWS EC2 Mac instances]({{< relref "aws-ec2-mac/_index.md" >}}) started from the [Marketplace AMI]({{< relref "aws-ec2-mac/marketplace-ami.md" >}}). The AMI ships with a licensed [Anka Virtualization CLI]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}}) and the macOS tweaks we use for EC2 Mac. Ops does not install Anka or activate a BYOL license on each host.

The [Controller and Registry]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) run on a Linux EC2 instance in the same AWS region and VPC as the Mac nodes. Nodes and Registry reach each other over private IP addresses inside the VPC. Template pushes and tag pulls stay on that network instead of crossing the public internet, which keeps image transfer fast when brands publish or jobs pull large Xcode and simulator layers.

Jenkins, GitLab Runner, and GitHub Actions talk to the Controller through the [plugins]({{< relref "plugins-and-integrations/controller-+-registry/_index.md" >}}). Brands do not each install a different Mac virtualization product.

{{< hint info >}}
The [Marketplace AMI]({{< relref "aws-ec2-mac/marketplace-ami.md" >}}) bills Anka hourly through AWS while the instance runs. There is no separate annual license to renew or fulfillment ID to track on each node. [EC2 Mac nodes]({{< relref "aws-ec2-mac/_index.md" >}}) [join Build Cloud]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}) per the node prep guide. Ops adds instances before a store freeze, scales the pool down on weekends when CI is quiet, and stops instances after a freeze. The Controller, Registry, and template tags stay the same as the pool scales.
{{< /hint >}}

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Controller and Registry on EC2 in same VPC as EC2 Mac nodes]({{< siteurl >}}images/use-cases/physical-goods-enterprise/deployment-architecture.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Scale

<!-- REVIEW: scale -->
Tens of EC2 Mac instances in AWS. Concurrent VMs stay in the tens on a normal week and rise when a brand freezes. Ops stops most instances on Friday evening and starts them again Monday morning when weekday CI picks up.

Most compile jobs never need the full resources of the host. The Controller places two smaller VMs on a node when the job fits. iOS simulator test jobs run in a VM configured to consume the full CPU, memory, and display resources of the host. Store-freeze weeks add EC2 Mac instances to the same Controller. Brands keep a child tag per app, not a private Mac per region.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Two VMs per node versus one full-host VM by job size]({{< siteurl >}}images/use-cases/shared/release-scale.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## How Anka is used

### Build Cloud environment

Their [Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/_index.md" >}}) runs entirely in one AWS region. A Linux EC2 instance hosts the [Controller and Registry]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}). [EC2 Mac instances]({{< relref "aws-ec2-mac/_index.md" >}}) from the [Marketplace AMI]({{< relref "aws-ec2-mac/marketplace-ami.md" >}}) join the same VPC. Each Mac node already has the [Anka Virtualization CLI]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}}) licensed and the EC2 Mac disk and networking settings we ship in the AMI. The Controller schedules VMs across the pool. The Registry stores templates and tags; nodes pull them over the VPC private network.

### Template Catalog

Central IT publishes a base [VM template and tag]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}}) in the Registry: macOS, Xcode, and the iOS Simulator runtimes every brand must stay aligned on. Each brand adds its app signing, SDKs, and test fixtures as a child tag on top of the base. Together these form the Template Catalog.

Brand teams set the tag name in their CI configuration. Packer or CLI scripts keep the base tag current so every job uses the same Xcode and simulator versions.

That work is image creation, not day-to-day CI. Brands layer on the base. They do not fork a separate image pipeline per region.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Base and brand tags in Registry and brand CI job flow]({{< siteurl >}}images/use-cases/physical-goods-enterprise/job-lifecycle.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

### CI and jobs

1. CI requests a VM from the Controller for the brand’s configured tag.
2. The node pulls the tag from the Registry over a private IP and starts the VM.
3. The job runs inside the VM. Compile steps and iOS Simulator tests use the Xcode and simulator runtimes baked into the tag.
4. The VM is deleted when the job ends.

Before a store freeze, ops starts extra EC2 Mac instances and lets them pull the needed tags. After the freeze, ops drains and stops instances that are no longer needed. The base tag uses [CLI advanced security]({{< relref "anka-virtualization-cli/advanced-security-features.md" >}}): `--no-local` and [IP filtering]({{< relref "anka-virtualization-cli/getting-started/understanding-vm-networking.md#ip-filtering" >}}) (`block local`) so a brand job cannot talk to another VM or the host when several brands share a node.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![VM network isolation with block local and no-local]({{< siteurl >}}images/use-cases/physical-goods-enterprise/network-isolation.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Value

<!-- REVIEW: value -->
Mac CI is one EC2 Mac fleet instead of a pile of brand-owned minis. The Marketplace AMI removes annual license renewal and per-node activation work. Hourly billing matches usage: the pool scales up for store freezes and scales down on weekends when no one is building. Co-locating the Registry with Mac nodes in the same VPC keeps template push and pull fast over private network paths. Brands still own their app-specific tag, but they do not own a second hypervisor or a private image pipeline.

Simulator tests run inside a fresh VM with a known Xcode and runtime stack. Leftover simulator data does not carry to the next brand’s job the way it did on shared bare-metal Macs. Any node in the pool can pull the tag a job targets and run it.

## Practices that worked

- Publish one base tag from the center with Xcode and iOS Simulator runtimes. Let brands layer child tags, not fork the whole image.
- Run simulator tests inside the VM. Do not install simulators on the Mac node for CI jobs.
- Size simulator jobs for a full-host VM. Do not pack two simulator-heavy jobs on one node.
- Run the Controller and Registry on a Linux EC2 instance in the same region and VPC as the Mac nodes. Point nodes at the Registry private IP.
- Launch nodes from the [Marketplace AMI]({{< relref "aws-ec2-mac/marketplace-ami.md" >}}). Do not BYOL unless you have a reason to manage licenses yourself.
- Scale the EC2 Mac pool through the same Controller. Do not stand up a second cluster for freeze week.
- Stop EC2 Mac instances on weekends and after a freeze. Running instances still cost money.
- Standardize on Controller plugins even when CI products differ. The VM lifecycle stays the same.
- Put `--no-local` or IP filter `block local` on the shared base tag so brands on the same host cannot reach each other or the node.
- Delete the VM at the end of the job. Do not reuse a running VM across unrelated builds.

## Related docs

- [Getting Started (CLI)]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}})
- [Creating VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})
- [Setup Controller and Registry]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}})
- [Anka on AWS EC2 Macs]({{< relref "aws-ec2-mac/_index.md" >}})
- [Marketplace AMI]({{< relref "aws-ec2-mac/marketplace-ami.md" >}})
- [Controller plugins]({{< relref "plugins-and-integrations/controller-+-registry/_index.md" >}})
- [Packer]({{< relref "plugins-and-integrations/packer.md" >}})
- [GitLab Runner]({{< relref "plugins-and-integrations/controller-+-registry/gitlab-runner.md" >}})
- [Advanced Security Features (CLI)]({{< relref "anka-virtualization-cli/advanced-security-features.md" >}}) (`--no-local`, IP filtering)
- [VM networking]({{< relref "anka-virtualization-cli/getting-started/understanding-vm-networking.md" >}})
