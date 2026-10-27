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

Veertu provides AMIs with Anka Virtualization pre-installed and macOS pre-configured with our best practices. Use them to get started on AWS EC2 Mac instances (mac1 Intel and mac2+ Apple silicon) without purchasing and managing your own hardware.

With Anka on your EC2 Mac instance, you can:

- Run more than one ephemeral macOS VM on each instance to lower cost.
- Use the full first 24 hours that Amazon charges for each EC2 Mac instance.
- Scale VM capacity up and down with the AWS Management Console or AWS CLI.
- Prevent jobs from leaving a dirty environment for other jobs.
- Run multiple versions of macOS on the same host.

For more information, see [our site](https://veertu.com/aws-ec2-mac/) and [Amazon's blog](https://aws.amazon.com/blogs/compute/getting-started-with-anka-on-ec2-mac-instances/).

## Before you start

- **Dedicated host:** Request a dedicated mac\* host to run EC2 Mac instances. Requesting, stopping, and starting instances takes time because the host cleans itself each time an instance stops.
- **Disk cache setting (Anka 3.9.3+):** Set `block_nocache` to `2` for your user and for root. Our latest AMIs already include this. If you upgrade Anka on an older AMI or instance, run:

    ```bash
    anka config block_nocache 2 && sudo anka config block_nocache 2
    ```

- **Anka Build Cloud (optional):** You do not need the Anka Build Cloud to run Anka VMs on EC2 Macs. You need it only to create VMs with our [Controller Plugins/Integrations]({{< relref "plugins-and-integrations/controller-+-registry/_index.md" >}}). We do not provide an AMI for it. [Set it up]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) on a Linux server or Docker container in AWS, not on the EC2 Mac instances.

## Choose your AMI

| | [Marketplace AMI]({{< relref "marketplace-ami.md" >}}) | [Community AMI (BYOL)]({{< relref "community-ami.md" >}}) | [Build your own](#build-your-own-ami) |
|---|---|---|---|
| **Anka pre-installed** | Yes | Yes | No |
| **macOS pre-optimized** | Yes | Yes | No |
| **License** | Not needed. Hourly billing through AWS. | Your own Anka license (Anka Develop does not work) | Your own Anka license (Anka Develop does not work) |
| **`block_nocache 2` set** | Yes (latest AMIs) | Yes (latest AMIs) | You must set it |
| **Requirements** | Instance metadata service enabled | — | gp3 EBS volume, 6000 IOPS, 256 throughput |

## Build your own AMI

You can review our [AMI scripts](https://github.com/veertuinc/aws-ec2-mac-amis) to see how we build our AMIs.

1. Use an EBS volume with a minimum of gp3, 6000 IOPS, and 256 throughput. Anka VM creation is sensitive to slow disks and will likely fail on them.
2. Install Anka and activate your license.
3. Set the disk cache setting before you create the AMI image:

    ```bash
    anka config block_nocache 2 && sudo anka config block_nocache 2
    ```

4. Create the AMI image.

## Answers to Frequently Asked Questions

### How do I give VMs access to the instance metadata service (`169.254.169.254`)?

Route it from the host:

```bash
networksetup -setadditionalroutes Ethernet 169.254.169.254 255.255.255.255 \
  $(sudo defaults read /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist Shared_Net_Address)
```
