---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 3
description: How to upgrade the Anka Virtualization package
---

### Upgrade Procedure

1. [Download and install the latest version]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}})

{{< hint info >}}
Upgrading the Build Cloud too? Check out our [upgrade procedure for the Anka Build Cloud Controller & Registry]({{< relref "Anka Build Cloud/upgrading.md" >}})
{{< /hint >}}

{{< hint warning >}}
We do not follow strict [semantic versioning](https://semver.org/); minor and major version increases can have significant changes
{{< /hint >}}

{{< hint info >}}
Upgrading Anka Virtualization software while VMs are running **is typically safe.** Please see the Pre-upgrade Considerations below to be sure.
{{< /hint >}}

{{< hint info >}}
Upgrading the VM macOS version from within (using `softwareupdate`) typically works.
{{< /hint >}}

### Pre-upgrade Considerations

Existing Version | Target Version | Recommendation
--- | --- | ---
1.4.3 | 2.x.x | Requires upgrade of all existing VM templates with `anka start --update` and push to the registry
2.x | 2.1.2 | Only requires upgrade of existing Catalina VM templates with `anka start --update` and push to the registry
< 2.4 | >= 2.4 | Full feature support requires upgrading addons.
2.x | 2.5.x | <ul><li>macOS 10.14.x does not contain the necessary Apple APIs for Nested Virtualization to work on 2.5.x of Anka. If you use Nested Virtualization, you will need to upgrade the host OS version before trying to install 2.5.x of Anka.</li><li>Avoid upgrading the anka package to 2.5.X on nodes with VMs running.</li></ul>
< 2.5.x | 2.5.x | Suspended VMs < 2.5.x are not compatible with 2.5.x and will need to be force stopped (`anka stop --force`), started, and then re-suspended post-upgrade.

---

Existing VM macOS Version | Target Version | Recommendation
--- | --- | ---
<= Big Sur | Monterey | You will need to change your VM template: `anka modify {vmName} set hard-drive -c sata`, `anka modify {vmName} set network-card -c virtio-net`
<= Big Sur | Ventura | You will need to change your VM template: `anka modify {vmName} set hard-drive -c sata`, `anka modify {vmName} set network-card -c virtio-net`, and `anka modify {vmName} set custom-variable hw.x2apic 1`
Monterey | Ventura | You will need to change your VM template: `anka modify {vmName} set custom-variable hw.x2apic 1`

### Upgrading VM Addons

If your existing version is noted in the [Pre-Upgrade Considerations]({{< relref "intel/upgrading.md#pre-upgrade-considerations" >}}) as requiring an addons upgrade, or, if you're going between major/minor versions of Anka:

   1. Upgrade the guest addons inside existing VM templates with `anka start -u`
   2. Push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <newTag>`

{{< hint warning >}}
We cannot guarantee that suspended VMs will start properly between major and minor versions of Anka, though, we do try and ensure the compatibility.
{{< /hint >}}
