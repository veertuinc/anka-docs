---
date: 2017-08-16T14:27:24-07:00
title: "Licensing"
linkTitle: "Licensing"
weight: 10
description: >
  Detailed information on Anka Licensing
---

{{< hint info >}}
With the Anka M1 version, licensing of cores will be determined based on the performance core count.
{{< /hint >}}

---
---

{{< include file="_partials/Licensing/activating.md" >}}

---

Anka licenses are available for the following products:

+ **[Anka Develop]({{< relref "anka-develop.md" >}})** - Very limited features allowing a developer to run a single VM at a time. Supported only on laptops (Macbook, Macbook Pro, and Macbook Air).
+ **[Anka Build Basic]({{< relref "Anka Build Cloud/build-license-tiers.md" >}})** - All Basic features to configure and run macOS CI Cloud infrastructure.
+ **[Anka Build Enterprise]({{< relref "Anka Build Cloud/build-license-tiers.md" >}})** - Basic + additional features for grouping, priority provisioning, etc.
+ **[Anka Build Enterprise Plus]({{< relref "Anka Build Cloud/build-license-tiers.md" >}})** - Enterprise + additional features to support SSO and event logging.
+ **[Anka Flow]({{< relref "anka-flow.md" >}})** - Install and configure Anka on developer mac workstations. Supported only on macbook models and iMac.

## Trial License

Trial licenses for Anka products is valid for 30 days from the date of trial registration. They enable unlimited access to product features. You can use the same activation key on multiple machines depending upon the quantity specified at the time of trial registration. They can be obtained at https://veertu.com/getting-started-anka-trials/.

## Anka Virtualization

{{< include file="_partials/Licensing/feature-differences.md" >}}

## Anka Build Cloud

{{< include file="_partials/Licensing/build-tiers.md" >}}

---

## Commercial Licenses

> Commercial Anka licenses are issued for yearly license subscription which includes updates, upgrades, and standard support. They can be purchased for a single year or multiple years.

> Commercial licenses are automatically extended in activated Anka software if days to expiration is < 30 days. The machine where you've installed Anka must have a connection to the licensing server.

### Pricing

+ **Anka Build** - Cost is performance core based. For example, if you are setting up a cloud consisting of 2, 8-core ARM mac minis, then total performance core count will be 8 cores. For very large core count, there are other licensing models.
+ **Anka Flow** - Cost is per machine. For example, if you are installing Anka Flow on 10 developer machines, then quantity will be 10.

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

