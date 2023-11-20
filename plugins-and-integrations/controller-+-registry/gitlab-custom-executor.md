---
date: 2019-07-03T22:24:47-05:00
title: "Using GitLab with the Anka Build Cloud"
linkTitle: "Gitlab Executor"
weight: 3
description: >
  Instructions on how to use the Anka Cloud Gitlab Executor with the Anka Build Cloud
---

## VM Template & Tag Requirements

> The below list are the absolute necessities needed to execute commands in a VM through your CI and the GitLab Runner. You may have to include other dependencies depending on your needs.

1. In the VM:
    - Install `git`
    - Make sure remote login is enabled (`System Preferences > Sharing`).
2. On the host, enable [port forwarding]({{< relref "anka-virtualization-cli/command-line-reference.md#modify-nameuuid-port" >}}) of the VM's 22 port using the Anka CLI. _We recommend not specifying `--host-port`._
3. Push the VM as a Template/Tag to the Anka Build Registry.

## Installation of the Anka Cloud Gitlab Executor

Please see https://github.com/veertuinc/anka-cloud-gitlab-executor for deployment instructions and release notes.
