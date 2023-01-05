---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 5
description: How to upgrade the Anka Virtualization package
---

{{< hint info >}}
Upgrading the Build Cloud too? Check out our [upgrade procedure for the Anka Build Cloud Controller & Registry]({{< relref "anka-build-cloud/upgrading.md" >}})
{{< /hint >}}

{{< hint warning >}}
We do not follow strict [semantic versioning](https://semver.org/); minor and major version increases can have significant changes
{{< /hint >}}

### Pre-upgrade Considerations

Existing Version | Target Version | Recommendation
--- | --- | ---
<= 2.x.x | 3.2.0 | 

Existing VM macOS Version | Target Version | Recommendation
--- | --- | ---
<= Big Sur | Monterey | You will need to change your VM template: `anka modify {vmName} set hard-drive -c sata`, `anka modify {vmName} set network-card -c virtio-net`
<= Big Sur | Ventura | You will need to change your VM template: `anka modify {vmName} set hard-drive -c sata`, `anka modify {vmName} set network-card -c virtio-net`, and `anka modify {vmName} set custom-variable hw.x2apic 1`
Monterey | Ventura | You will need to change your VM template: `anka modify {vmName} set custom-variable hw.x2apic 1`

### Upgrade Procedure

#### [1. Download and install the latest version]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})

{{< hint info >}}
Upgrading the Build Cloud too? Check out our [upgrade procedure for the Anka Build Cloud Controller & Registry]({{< relref "anka-build-cloud/upgrading.md" >}})
{{< /hint >}}

{{< hint warning >}}
We do not follow strict [semantic versioning](https://semver.org/); minor and major version increases can have significant changes
{{< /hint >}}

{{< hint info >}}
Upgrading Anka Virtualization software while VMs are running **is typically safe.** Please see the Pre-upgrade Considerations below to be sure.
{{< /hint >}}

#### 2. Upgrade VM addons

If your existing version is noted in the [Pre-Upgrade Considerations]({{< relref "anka-virtualization-cli/upgrading.md#pre-upgrade-considerations" >}}) as requiring an addons upgrade, or, if you're going between major/minor versions of Anka:

   1. Upgrade the guest addons inside existing VM Templates `anka start -u`. This opens the VM desktop in a viewer window and allows you to manually launch and install the addons package.
   2. Push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <newTag>`
