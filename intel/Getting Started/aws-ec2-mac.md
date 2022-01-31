---
title: "Running on AWS EC2 Mac"
linkTitle: "Running on AWS EC2 Mac"
weight: 8
date: 2017-01-20
description: >
  Up and running fast, with Amazon (AWS) EC2 Macs
---

{{< hint warning >}}
**Our AMIs are owned primarily by account ID 930457884660, however, some older AMIs were created under 458569289506. We do not recommend trusting any other Anka AMIs**
{{< /hint >}}

Customers often find that purchasing and managing their own hardware can become a burden. This is why we recommend using AWS EC2 Mac instances to Anka Virtualization software.

With Anka installed on your AWS EC2 Mac instance, you can run ephemeral macOS VMs as well as optimize the instance cost by running more than one at a time. [Visit our site](https://veertu.com/aws-ec2-mac/) and [Amazon's blog](https://aws.amazon.com/blogs/compute/getting-started-with-anka-on-ec2-mac-instances/) for more information about AWS EC2 Mac and Anka.

There are three options available for you to use Anka with AWS EC2 Mac instances:

1. [Use our Marketplace AMI]({{< relref "#marketplace-ami" >}}) (macOs pre-configured/optimized + Anka installed)
2. [User our Licenseless AMI]({{< relref "#licenseless-community-ami" >}}) (macOS pre-configured/optimized + Anka installed)
3. [Build your own AMI]({{< relref "#build-your-own-ami" >}})

{{< hint info >}}
Note: Dedicated requests in AWS can take a while to become Available.
{{< /hint >}}

---

## Getting Started scripts

We have two scripts that will set up both a Linux instance with the [Anka Build Cloud Controller & Registry](https://veertu.com/anka-build/) as well as an EC2 Mac instance (Anka Node) to run VMs. You can find them under our [Getting Started repo’s AWS folder](https://github.com/veertuinc/getting-started#aws-aws).

1. Clone the getting-started repo

    ```bash
    git clone https://github.com/veertuinc/getting-started.git
    cd getting-started
    ```

2. Execute [`./AWS/prepare-build-cloud.bash`](https://github.com/veertuinc/getting-started/blob/master/AWS/prepare-build-cloud.bash)
    - Running this script will create everything necessary inside of AWS to run the Anka Build Cloud. This includes a security group, elastic IP, etc.

3. Execute [`./AWS/prepare-anka-node.bash`](https://github.com/veertuinc/getting-started/blob/master/AWS/prepare-anka-node.bash)

    - Requires that you first run `prepare-build-cloud.bash`. Otherwise, you need to set several necessary environment variables before execution. These ENVs can be found in the script under the comment `# Collect all existing ids and instances`.

    - Running this script will create everything necessary inside of AWS to run an EC2 Mac instance. You’ll be prompted for the Anka license to use if the ANKA_LICENSE env variable is not set.

Both scripts can be run locally from your local macOS laptop with an existing AWS credential, region set, etc. These scripts have not been tested on linux.

---

## Marketplace AMI

### Usage

To get up and running with our AWS EC2 Mac instances using our Marketplace AMI, you'll need to:

1. 

{{< include file="_partials/intel/Getting Started/_aws-ec2-mac-prep-steps.md" >}}

#### Licensing

The Marketplace AMI does not require a license. You are charged hourly for the usage through [the AWS marketplace](https://docs.aws.amazon.com/marketplace/latest/userguide/pricing.html).

{{< hint info >}}
Stopping and starting the instance does not impact the Anka licenses validity, even if you start the instance on a different dedicated machine.
{{< /hint >}}

---

## Licenseless Community AMI

Our Licenseless Community AMIs are useful if you'd like to use an existing Anka core or host based license. They both have all of the same configuration changes, optimizations, and Anka inside. The difference is that Anka is unlicensed.

{{< hint info >}}
Anka Develop license type does not support Mac Minis and will not work on EC2 Macs.
{{< /hint >}}

### Usage

To get up and running with our AWS EC2 Mac instances using our Licenseless Community AMI, you'll need to:

1. Have an AWS mac1 dedicated host ready.

2. Have an [Anka license](https://veertu.com/anka-build-trial/).

3. Choose the Community AMI when starting an instance:
  {{< imgwithlink src="images/getting-started/aws-ec2-mac/aws-choose-ami.png" >}}

{{< include file="_partials/intel/Getting Started/_aws-ec2-mac-prep-steps.md" >}}

#### Licensing

When you first license Anka, keep track of the fulfillment ID as you'll need this to release the cores and use the license on a fresh machine.

{{< hint info >}}
Stopping and starting the instance does not impact the Anka licenses validity, even if you start the instance on a different dedicated machine.
{{< /hint >}}

---

## Build your own AMI

Building your own AMI is easy! You can review our [AMI scripts](https://github.com/veertuinc/aws-ec2-mac-amis) to see how we do it.

---