---
date: 2017-08-16T14:27:24-07:00
title: "Licensing"
linkTitle: "Licensing"
weight: 10
description: >
  Detailed information on Anka Licensing
---

{{< include file="shared/content/en/docs/Licensing/partials/_activating.md" >}}

---

Anka licenses are available for the following products:

+ **[Anka Develop]({{< relref "docs/Anka Develop/_index.md" >}})** - Very limited features allowing a developer to run a single VM at a time.
+ **[Anka Build Basic]({{< relref "docs/Anka Build Cloud/license-tiers.md" >}})** - All Basic features to configure and run macOS CI Cloud infrastructure
+ **[Anka Build Enterprise]({{< relref "docs/Anka Build Cloud/license-tiers.md" >}})** - Basic + additional features for grouping, priority provisioning etc
+ **[Anka Build Enterprise Plus]({{< relref "docs/Anka Build Cloud/license-tiers.md" >}})** - Enterprise + additional features to support SSO, VM encryption, event logging
+ **[Anka Flow]({{< relref "docs/Anka Flow/_index.md" >}})** - Install and configure Anka on developer mac workstations. Supported only on macbook models and iMac.
+ **[Anka Secure]({{< relref "docs/Anka Secure/_index.md" >}})** - Install and configure Anka on mac workstations to create secure, sandbox macOS working environments for privileged data access etc

## Obtaining a trial license

Trial licenses for Anka products is valid for 30 days from the date of trial registration. They enable unlimited access to product features. You can use the same activation key on multiple machines depending upon the quantity specified at the time of trial registration. They can be obtained at [https://veertu.com/getting-started-anka-trials/](https://veertu.com/getting-started-anka-trials/).

## Anka License Feature Differences

| **Feature** | **Develop** | **Build** | **Flow** | **Secure** |
| --- | --- | --- | --- | --- |
| Run Multiple VMs | No | Yes | Yes | Yes |
| State Snapshot / Suspend VM | No | Yes | Yes | Yes |
| USB Device Support | No | Yes | Yes | Yes |
| Ability to communicate with Anka Build Cloud (Controller & Registry) | No | Yes | No | Yes |
| Runs on all macOS hardware models | No (Macbook and iMac only) | Yes | No (Macbook and iMac only) | Yes |

## Anka Build License Feature Differences

**Feature** | **Basic** | **Enterprise** | **Enterprise Plus**
--- | --- | --- |  ---
Core based licensing | Yes | Yes | Yes
Cloud Controller with REST APIs | Yes(Single instance of Anka controller included) | Yes(Single instance of Anka controller included) | Yes
Central Registry | Yes(Single instance Anka Registry included) | Yes | Yes
[GitHub Action](https://github.com/marketplace/actions/anka-vm-github-action) | Yes | Yes | Yes
[Jenkins Plugin](https://plugins.jenkins.io/anka-build/) | Yes | Yes | Yes
[TeamCity Plugin](https://plugins.jetbrains.com/plugin/10733-anka-build-cloud) | Yes | Yes | Yes
[GitLab Runner with custom executor](https://github.com/veertuinc/gitlab-runner) | Yes | Yes | Yes
[BuildKite Plugin](https://github.com/veertuinc/anka-buildkite-plugin) | Yes | Yes | Yes
HA for Controller configuration setup | Yes (Additional controller/registry instances needed) | Yes | Yes
[USB Device control through the CLI]({{< relref "docs/Anka Virtualization/working-with-usb-devices.md" >}}) |  Yes  | Yes | Yes
[USB Device control through Controller API]({{< relref "#usb-device-control-controller-api" >}}) |    | Yes | Yes
[Priority scheduling of VMs through controller]({{< relref "#priority-scheduling" >}}) |    | Yes | Yes
[Clustering (Grouping) of Nodes]({{< relref "#node-groups" >}}) |    | Yes | Yes 
[Basic controller authentication (Certificate & Root Superuser Token)]({{< relref "docs/Anka Build Cloud/Advanced Security Features/_index.md" >}}) |    | Yes | Yes
[Multi-user & group authorization with admin panel + OpenID/SSO support]({{< relref "docs/Anka Build Cloud/Advanced Security Features/openid-authentication.md" >}}) |    |    | Yes
Controller API activity logging |    |    | Yes
VM full disk encryption for Build VMs |    |    | Yes
Control VM runtime (Networking, Access to host) and functional properties with policies |    |    | Yes

---

## Commercial Licenses

> Commercial Anka licenses are issued for yearly license subscription which includes updates, upgrades, and standard support. They can be purchased for a single year or multiple years.

> Commercial licenses are automatically extended in activated Anka software if days to expiration is < 30 days. The machine where you've installed Anka must have a connection to the licensing server.

### Pricing

+ **Anka Build** - Cost is core based. For example, if you are setting up a cloud consisting of 2 6-core mac minis, then total core count will be 12 cores.
+ **Anka Flow** - Cost is per machine. For example, if you are installing Anka Flow on 10 developer machines, then quantity will be 10.
+ **Anka Secure** - Cost is per machine. For example, if you are installing Anka Flow on 10 mac workstations, then quantity will be 10.

Contact [support@veertu.com](mailto:support@veertu.com) to get a pricing estimate.  

### Upgrading your Trial license to a Commercial license

1. Stop all the running or suspended VMs. 
2. Remove the old license and activate with the commercial activation key.

    ```shell
    sudo anka license remove
    sudo anka license activate {LICENSE}
    ```

### Moving your Commercial license from one machine to another

1. Remove the license from the machine using the below described command.

    ```shell
    sudo anka license remove
    Are you sure you want to remove current license? [y/N]: y
    License is removed. To activate with the same activation key on another machine,
    contact support@veertu.com with your activation key and fulfillment ID 2727000039
    ```

2. Contact [support@veertu.com](mailto:support@veertu.com) with the fulfillment ID provided in the output of the `anka license remove` command and your license key.




