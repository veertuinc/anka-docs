---
title: "Running on AWS EC2"
linkTitle: "Running on AWS EC2"
weight: 8
date: 2017-01-20
description: >
  Up and running fast, with Amazon (AWS) EC2
---

Customers often find that purchasing and managing their own hardware can become a burden. This is why we recommend using AWS EC2 Mac instances to Anka Virtualization software.
With Anka installed on your AWS EC2 Mac instance, you can run ephemeral macOS VMs as well as optimize the instance cost by running more than one at a time. [Visit our site](https://veertu.com/aws-ec2-mac/) for more information about AWS EC2 Mac and Anka.

---

To get up and running quickly with AWS EC2 Mac instances, you first need a trial license. Once you have your license, we have scripts that will set up the complete orchestration. You can find them under our [Getting Started repo’s AWS folder](https://github.com/veertuinc/getting-started#aws-aws). There are two scripts available:

### [prepare-build-cloud.bash](https://github.com/veertuinc/getting-started/blob/master/AWS/prepare-build-cloud.bash)

Running this script will create everything necessary inside of AWS to run an Anka Build Cloud. This includes a security group, elastic IP, etc.

### [prepare-anka-node.bash](https://github.com/veertuinc/getting-started/blob/master/AWS/prepare-anka-node.bash)

> Requires you first run `prepare-build-cloud.bash`. If you already have a EC2 Mac instance you can use the scripts in [https://github.com/veertuinc/aws-ec2-mac-amis] instead. 

Running this script will create everything necessary inside of AWS to run a mac1.metal instance and then install Anka inside. You’ll be prompted for the Anka license to use if the ANKA_LICENSE env variable is not set.
Both scripts can be run locally from your macOS hardware with an existing AWS credential.