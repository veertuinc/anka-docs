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

1. [**Use our Marketplace AMI**]({{< relref "#marketplace-ami" >}})
  - macOs pre-configured/optimized + Anka installed
  - Provides an hourly billing option for Anka based on the uptime of your EC2 Mac instance
2. [**Bring your own license (BYOL) Community AMI**]({{< relref "#community-ami" >}})
  - macOS pre-configured/optimized + Anka installed
  - With these AMIs, you will be able to use your own Anka License.
3. [**Build your own AMI**]({{< relref "#build-your-own-ami" >}})

{{< hint info >}}
Note: You must request a dedicated mac* host in order to run EC2 Mac instances. There is a known delay requesting, stopping, and starting EC2 Mac instances as the dedicated host must clean itself each time an instance stops on it.
{{< /hint >}}

---

## Marketplace AMI

In order to get started using our Marketplace AMIs you have four options:

1. Basic License: [intel](https://aws.amazon.com/marketplace/pp/prodview-tzmid5kfn2knw) | [arm64](https://aws.amazon.com/marketplace/pp/prodview-n54do7hhvqd2q)
2. Enterprise License: [intel](https://aws.amazon.com/marketplace/pp/prodview-rp7kfyl56arbe) | [arm64](https://aws.amazon.com/marketplace/pp/prodview-w6sxovunownv6)
3. Enterprise Plus License: [intel](https://aws.amazon.com/marketplace/pp/prodview-qs6yqgdbmc4vm) | [arm64](https://aws.amazon.com/marketplace/pp/prodview-td6ik34jd2z2m)

Other than the hourly price, there is [a list of features that differ between the two.]({{< relref "licensing.md#anka-build-cloud" >}})

{{< hint info >}}
You can find a full list of products available on the AWS marketplace by visiting https://veertu.com/aws-marketplace. Or, once subscribed, you can find and launch instances from the marketplace AMIs on the [Manage Subscriptions page](https://console.aws.amazon.com/marketplace/home/subscriptions?#/subscriptions).
{{< /hint >}}

{{< hint info >}}
Marketplace AMIs are charged on an hourly basis and don't need an Anka License.
{{< /hint >}}

{{< hint info >}}
You can create custom AMIs from the Marketplace AMI and the license for Anka will continue to work and attach to your existing marketplace subscription.
{{< /hint >}}

### Usage

To get up and running with our AWS EC2 Mac instances using our Marketplace AMI, you'll need to navigate to one of the Marketplace AMI Product URLs listed above and go through the process of subscribing. [Take a look at the official AMI Subscription documentation to understand how to subscribe.](https://docs.aws.amazon.com/marketplace/latest/buyerguide/buyer-ami-subscriptions.html)

Once subscribed, you can start launching AMIs.

{{< include file="_partials/ec2-mac-amis/prep-steps.md" >}}

#### Licensing

The Marketplace AMI does not require a license. You are charged hourly for the usage through [the AWS marketplace](https://docs.aws.amazon.com/marketplace/latest/userguide/pricing.html). Anka marketplace AMIs are available with Anka Basic and Anka Enterprise Tier features. For more details on Basic and Enterprise Tier, [check out our documentation.]({{< relref "licensing.md#anka-build-cloud" >}})

### Anka Build Cloud automated setup scripts

We have a script that will set up both a Linux instance with the [Anka Build Cloud Controller & Registry](https://veertu.com/anka-build/). You can find it under our [Getting Started repo’s AWS folder](https://github.com/veertuinc/getting-started#aws-aws).

1. Clone the getting-started repo

    ```bash
    git clone https://github.com/veertuinc/getting-started.git
    cd getting-started
    ```

2. Execute [`./AWS/prepare-build-cloud.bash`](https://github.com/veertuinc/getting-started#prepare-build-cloudbash)
    - Running this script will create everything necessary inside of AWS to run the Anka Build Cloud. This includes a security group, elastic IP, etc.

The script can be run locally from your local macOS laptop with an existing AWS credential, region set, etc. These scripts have not been tested on linux.

---

## Community AMI

Our BYOL Community AMIs are useful if you'd like to bring your own existing Anka license. They both have all of the same configuration changes, optimizations, and Anka inside. The difference is that Anka is unlicensed.

You can find a list of currently available Community AMIs below:

{{< include file="_partials/ec2-mac-amis/community-ami-list.md" >}}

### Usage

To get up and running with our AWS EC2 Mac instances using our BYOL Community AMI, you'll need to:

1. Have an AWS mac1 (intel) or mac2 (arm/apple/m1) dedicated host ready.

2. Have an [Anka license](https://veertu.com/anka-build-trial/).

3. Choose the Community AMI when starting an instance:
  {{< imgwithlink src="images/getting-started/aws-ec2-mac/aws-choose-ami.png" >}}

{{< include file="_partials/ec2-mac-amis/prep-steps.md" >}}

#### Licensing

When you first license Anka, keep track of the fulfillment ID as you'll need this to release the cores and use the license on a fresh machine.

{{< hint info >}}
The Anka Develop license type will not work on AWS EC2 Macs.
{{< /hint >}}

{{< hint info >}}
Stopping and starting the instance does not impact the Anka licenses validity, even if you start the instance on a different dedicated machine.
{{< /hint >}}

{{< hint warning >}}
Before terminating an instance, you will need to remove the Anka license first and then contact Veertu support (support@veertu.com) to clear the fulfillments
{{< /hint >}}

### Anka Build Cloud automated setup scripts

We have two scripts that will set up both a Linux instance with the [Anka Build Cloud Controller & Registry](https://veertu.com/anka-build/) as well as an EC2 Mac instance (Anka Node) to run VMs. This relies on our Community AMI and you will need to have an Anka License. You can find them under our [Getting Started repo’s AWS folder](https://github.com/veertuinc/getting-started#aws-aws).

1. Clone the getting-started repo

    ```bash
    git clone https://github.com/veertuinc/getting-started.git
    cd getting-started
    ```

2. Execute [`./AWS/prepare-build-cloud.bash`](https://github.com/veertuinc/getting-started#prepare-build-cloudbash)
    - Running this script will create everything necessary inside of AWS to run the Anka Build Cloud. This includes a security group, elastic IP, etc.

3. Execute [`./AWS/prepare-anka-node.bash`](https://github.com/veertuinc/getting-started#prepare-anka-nodebash)

    - Requires that you first run `prepare-build-cloud.bash`.

    - Running this script will create everything necessary inside of AWS to run an EC2 Mac instance. You’ll be prompted for the Anka license to use if the ANKA_LICENSE env variable is not set.

Both scripts can be run locally from your local macOS laptop with an existing AWS credential, region set, etc. These scripts have not been tested on linux.

---

## Build your own AMI

Building your own AMI is easy! You can review our [AMI scripts](https://github.com/veertuinc/aws-ec2-mac-amis) to see how we do it.

Some important notes about creating your own AMI:

- Be sure that the minimum EBS volume specs are gp3, 6000IOPS, and 256 throughput. Anka VM creation is sensitive on slow disks and will likely fail.
- If using the Anka Build Cloud: This step requires that you first [set up the Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) on a Linux server/docker container in AWS (but not on the EC2 Mac instances we provide AMIs for).

---

## Answers to Frequently Asked Questions

- VMs can enable access to the `169.254.169.254` instance Metadata by routing it from the host with: `networksetup -setadditionalroutes Ethernet 169.254.169.254 255.255.255.255 $(sudo defaults read /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist Shared_Net_Address)`
