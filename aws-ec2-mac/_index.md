---
title: "Anka on AWS EC2 Macs"
linkTitle: "Anka on AWS EC2 Macs"
weight: 5
date: 2017-01-20
description: >
  Up and running fast, with Anka and Amazon (AWS) EC2 Macs
aliases:
  - /anka-virtualization-cli/getting-started/aws-ec2-mac/
---

{{< hint info >}}
This guide is also valid for the Anka 3/mac2.metal/Apple processor (M1, M2, etc) EC2 instances.
{{< /hint >}}

{{< hint info >}}
While you do not need to use the Anka Build Cloud with AWS EC2 Mac instances to run Anka VMs, it is a requirement if you'd like to use our [Controller Plugins/Integrations]({{< relref "plugins-and-integrations/controller-+-registry/_index.md" >}}) to create VMs. You can read more about the Anka Build Cloud [here]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}).
{{< /hint >}}

Customers often find that purchasing and managing their own hardware can become a burden. While we do not currently provide an AMI for the [Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}), we do for the Anka Virtualization software. This makes it easy to get started with AWS EC2 Mac instances that you can scale as needed using the AWS Management Console or AWS CLI. The AMI is pre-prepared with best practices/configurations for using Anka.

With Anka installed on your AWS EC2 Mac instance, you can:

1. Run ephemeral macOS VMs as well as optimize the instance cost by running more than one at a time. 
2. Avoid idle hosts and maximize the usage in the first 24 hour period that Amazon charges you for when starting an EC2 Mac instance.
3. Scale VM capacity up and down as needed using the AWS Management Console or AWS CLI.
4. Prevent jobs from leaving a dirty environment on the host for other jobs to conflict with.
5. Run multiple versions of macOS on the same host.

And much more! [Visit our site](https://veertu.com/aws-ec2-mac/) and [Amazon's blog](https://aws.amazon.com/blogs/compute/getting-started-with-anka-on-ec2-mac-instances/) for more information about AWS EC2 Mac and Anka.

There are three AMI options available for you to use:

1. [**Use our Marketplace AMI**]({{< relref "marketplace-ami.md" >}})
  - macOs pre-configured/optimized + Anka installed
  - Provides an hourly billing option for Anka based on the uptime of your EC2 Mac instance
  - **Marketplace AMIs require `Instance metadata service` to be enabled.**
2. [**Bring your own license (BYOL) Community AMI**]({{< relref "community-ami.md" >}})
  - macOS pre-configured/optimized + Anka installed
  - With these AMIs, you will be able to use your own Anka License.
3. [**Build your own AMI**]({{< relref "#build-your-own-ami" >}})

{{< hint info >}}
Note: You must request a dedicated mac* host in order to run EC2 Mac instances. There is a known delay requesting, stopping, and starting EC2 Mac instances as the dedicated host must clean itself each time an instance stops on it.
{{< /hint >}}

---

## Build your own AMI

Building your own AMI is easy! You can review our [AMI scripts](https://github.com/veertuinc/aws-ec2-mac-amis) to see how we do it.

Some important notes about creating your own AMI:

- Be sure that the minimum EBS volume specs are gp3, 6000IOPS, and 256 throughput. Anka VM creation is sensitive on slow disks and will likely fail.
- If using the Anka Build Cloud: This step requires that you first [set up the Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) on a Linux server/docker container in AWS (but not on the EC2 Mac instances we provide AMIs for).

---

## Answers to Frequently Asked Questions

- VMs can enable access to the `169.254.169.254` instance Metadata by routing it from the host with: `networksetup -setadditionalroutes Ethernet 169.254.169.254 255.255.255.255 $(sudo defaults read /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist Shared_Net_Address)`

