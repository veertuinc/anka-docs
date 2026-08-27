---
date: 2026-08-25T00:00:00-00:00
title: "Use Cases"
linkTitle: "Use Cases"
weight: 7
description: >
  macOS CI with Anka Use Cases
---

Organizations run Anka with the same building blocks: the [Virtualization CLI]({{< relref "anka-virtualization-cli/_index.md" >}}), an optional [Build Cloud Controller and Registry]({{< relref "anka-build-cloud/_index.md" >}}), and [CI plugins]({{< relref "plugins-and-integrations/_index.md" >}}). The constraints change. A CI provider needs isolation between tenants. A bank needs access control on a private network. A game studio needs stable Xcode build times. The pages below show how those setups differ.

## Examples

### Gaming

Engine and editor CI at high parallel volume. Controller and Registry on a large node pool. Typical scale: tens to low hundreds of nodes. [Full example]({{< relref "use-cases/gaming.md" >}}).

iOS and macOS game-client builds across several teams. Shared templates with isolation so one title does not block another. Typical scale: dozens of nodes. [Full example]({{< relref "use-cases/gaming-studio.md" >}}).

### CI service provider

macOS GitHub-style runners on a custom orchestrator. CLI-only Anka with export/import; no Controller or Registry. Typical scale: hundreds of concurrent VMs. [Full example]({{< relref "use-cases/ci-service-provider.md" >}}).

### Regulated Sector

On-prem macOS build and test with Anklet, GitHub Actions, and a standalone Registry. No Controller. Typical scale: tens of nodes. [Full example]({{< relref "use-cases/regulated-sector.md" >}}).

### Enterprise using AWS EC2 Macs

EC2 Mac Marketplace AMI on AWS for enterprise iOS apps. Controller and Registry on Linux EC2 in the same VPC as Mac nodes; simulator tests inside VMs. Typical scale: multiple tens of instances, scaled down on weekends. [Full example]({{< relref "use-cases/enterprise-using-aws-ec2-macs.md" >}}).

## Start here

If you are new to Anka, install the CLI first, then add the Build Cloud if you need a shared node pool:

1. [Getting Started (CLI)]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}})
2. [Getting Started (Cloud)]({{< relref "anka-build-cloud/getting-started/_index.md" >}})
