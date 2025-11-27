---
title: "Community AMI (BYOL)"
linkTitle: "Community AMI (BYOL)"
weight: 2
description: >
  Use our Community AMI with your own Anka license
---

Our BYOL Community AMIs are useful if you'd like to bring your own existing Anka license. They both have all of the same configuration changes, optimizations, and Anka inside. The difference is that Anka is unlicensed.

You can find a list of currently available Community AMIs below:

{{< include file="_partials/ec2-mac-amis/community-ami-list.md" >}}

## Usage

To get up and running with our AWS EC2 Mac instances using our BYOL Community AMI, you'll need to:

1. Have an AWS mac1 (intel) or mac2 (arm/apple/m1) dedicated host ready.

2. Have an [Anka license](https://veertu.com/anka-build-trial/).

3. Choose the Community AMI when starting an instance:
  {{< imgwithlink src="images/getting-started/aws-ec2-mac/aws-choose-ami.png" >}}

{{< include file="_partials/ec2-mac-amis/prep-steps.md" >}}

### Licensing

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

## Anka Build Cloud automated setup scripts

We have two scripts that will set up both a Linux instance with the [Anka Build Cloud Controller & Registry](https://veertu.com/anka-build/) as well as an EC2 Mac instance (Anka Node) to run VMs. This relies on our Community AMI and you will need to have an Anka License. You can find them under our [Getting Started repo's AWS folder](https://github.com/veertuinc/getting-started#aws-aws).

1. Clone the getting-started repo

    ```bash
    git clone https://github.com/veertuinc/getting-started.git
    cd getting-started
    ```

2. Execute [`./AWS/prepare-build-cloud.bash`](https://github.com/veertuinc/getting-started#prepare-build-cloudbash)
    - Running this script will create everything necessary inside of AWS to run the Anka Build Cloud. This includes a security group, elastic IP, etc.

3. Execute [`./AWS/prepare-anka-node.bash`](https://github.com/veertuinc/getting-started#prepare-anka-nodebash)

    - Requires that you first run `prepare-build-cloud.bash`.

    - Running this script will create everything necessary inside of AWS to run an EC2 Mac instance. You'll be prompted for the Anka license to use if the ANKA_LICENSE env variable is not set.

Both scripts can be run locally from your local macOS laptop with an existing AWS credential, region set, etc. These scripts have not been tested on linux.

