---
title: "Marketplace AMI"
linkTitle: "Marketplace AMI"
weight: 1
description: >
  Use our AWS Marketplace AMI with hourly billing for Anka
---

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
You can create custom AMIs from the Marketplace AMI and the license for Anka will continue to work and attach to your existing marketplace subscription. However, if we remove the base AMI you took a snapshot from, it will show an expired license and stop working. We instead recommend using our base AMIs instead of creating your own.
{{< /hint >}}

## Usage

To get up and running with our AWS EC2 Mac instances using our Marketplace AMI, you'll need to navigate to one of the Marketplace AMI Product URLs listed above and go through the process of subscribing. [Take a look at the official AMI Subscription documentation to understand how to subscribe.](https://docs.aws.amazon.com/marketplace/latest/buyerguide/buyer-ami-subscriptions.html)

Once subscribed, you can start launching AMIs. **Marketplace AMIs require `Instance metadata service` to be enabled.**

{{< include file="_partials/ec2-mac-amis/prep-steps.md" >}}

### Licensing

The Marketplace AMI does not require a license. You are charged hourly for the usage through [the AWS marketplace](https://docs.aws.amazon.com/marketplace/latest/userguide/pricing.html). Anka marketplace AMIs are available with Anka Basic and Anka Enterprise Tier features. For more details on Basic and Enterprise Tier, [check out our documentation.]({{< relref "licensing.md#anka-build-cloud" >}})

## Anka Build Cloud automated setup scripts

We have a script that will set up both a Linux instance with the [Anka Build Cloud Controller & Registry](https://veertu.com/anka-build/). You can find it under our [Getting Started repo's AWS folder](https://github.com/veertuinc/getting-started#aws-aws).

1. Clone the getting-started repo

    ```bash
    git clone https://github.com/veertuinc/getting-started.git
    cd getting-started
    ```

2. Execute [`./AWS/prepare-build-cloud.bash`](https://github.com/veertuinc/getting-started#prepare-build-cloudbash)
    - Running this script will create everything necessary inside of AWS to run the Anka Build Cloud. This includes a security group, elastic IP, etc.

The script can be run locally from your local macOS laptop with an existing AWS credential, region set, etc. These scripts have not been tested on linux.

