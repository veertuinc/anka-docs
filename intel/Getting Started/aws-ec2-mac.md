---
title: "Running on AWS EC2"
linkTitle: "Running on AWS EC2"
weight: 8
date: 2017-01-20
description: >
  Up and running fast, with Amazon (AWS) EC2
---

Customers often find that purchasing and managing their own hardware can become a burden. This is why we recommend using AWS EC2 Mac instances to Anka Virtualization software.

With Anka installed on your AWS EC2 Mac instance, you can run ephemeral macOS VMs as well as optimize the instance cost by running more than one at a time. [Visit our site](https://veertu.com/aws-ec2-mac/) and [Amazon's blog](https://aws.amazon.com/blogs/compute/getting-started-with-anka-on-ec2-mac-instances/) for more information about AWS EC2 Mac and Anka.

## Usage

To get up and running with AWS EC2 Mac instances, you'll need to:

1. Have an [Anka License](https://veertu.com/anka-build-trial/).

2. Have an EC2 Mac instance which you can access through SSH and VNC.

### Community AMI

We highly recommend using our community AMIs as they contain a specific version of macOS and Anka installed and ready to license/use.

{{< imgwithlink src="images/getting-started/aws-ec2-mac/aws-choose-ami.png" >}}<br />

Our AMI attempts to do the majority of preparation for you, however, there are several steps you need to perform once the instance is started:

1. Set password with `sudo /usr/bin/dscl . -passwd /Users/ec2-user {NEWPASSWORDHERE}`.

2. You now need to VNC in once (requirement for Anka to start the hypervisor): `open vnc://ec2-user:{GENERATEDPASSWORD}@{INSTANCEPUBLICIP}`.

3. Once in VNC, Go to Preferences > Security > under General > uncheck require password after screensave or sleep begins option.

{{< hint info >}}
You can see how we generate these AMIs in our open source repo: https://github.com/veertuinc/aws-ec2-mac-amis
{{< /hint >}}

{{< hint info >}}
If you decided not to use our community AMIs, you'll need to [install Anka]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}}) just as you would with non-AWS hardware.
{{< /hint >}}

---

### Example Scripts

We also have scripts that will set up both a Linux host with the [Anka Build Cloud Controller & Registry](https://veertu.com/anka-build/) as well as an Anka Node to run VMs. You can find them under our [Getting Started repo’s AWS folder](https://github.com/veertuinc/getting-started#aws-aws).

1. Clone the getting-started repo
    ```bash
    git clone https://github.com/veertuinc/getting-started.git
    cd getting-started
    ```

2. Execute [`./AWS/prepare-build-cloud.bash`](https://github.com/veertuinc/getting-started/blob/master/AWS/prepare-build-cloud.bash)
    - Running this script will create everything necessary inside of AWS to run an Anka Build Cloud. This includes a security group, elastic IP, etc.

3. Execute [`./AWS/prepare-anka-node.bash`](https://github.com/veertuinc/getting-started/blob/master/AWS/prepare-anka-node.bash)

    - Requires that you first run `prepare-build-cloud.bash`. Otherwise, you need to set several necessary environment variables before execution. These ENVs can be found in the script under the comment `# Collect all existing ids and instances`.

    - Running this script will create everything necessary inside of AWS to run a mac1.metal instance and then install Anka inside. You’ll be prompted for the Anka license to use if the ANKA_LICENSE env variable is not set.

Both scripts can be run locally from your macOS hardware with an existing AWS credential, region set, etc.
