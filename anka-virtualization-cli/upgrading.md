---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading (CLI)"
linkTitle: "Upgrading"
weight: 5
description: How to upgrade the Anka Virtualization package
---

### Pre-upgrade Considerations

Existing Version | Target Version | Recommendation
| --- | --- | --- |
| 3.x | 3.3.9 | Please `kill -9` the `ankanetd` process after upgrading to 3.3.9. Older 3.x versions had a bug that kept `ankanetd` running indefinitely and will cause 3.3.9 to have networking issues trying to use the older `ankanetd`. |
| < 3.3 | 3.3 | Optional (ARM ONLY): Delete and re-pull VM templates to host machines to take advantage of new performance and smaller disk usage changes. |
| < 3.x | 3.x | {{< rawhtml >}}<ul><li>External volumes must be mounted as APFS in order to use them for `img_lib_dir`, etc.</li></ul>{{< /rawhtml >}} |
| < 3.x | 3.x (intel) | {{< rawhtml >}}<ul><li>VMs suspended on Anka 2.x will need to be re-suspended on 3.x.</li><li>The Controller Agent version that comes with Anka will start at <b>1.30.1</b>. <b>Older versions of the Controller do not support Anka 3. You will need to upgrade your controller to at least 1.30.1.</b></li><li>Previously, <code>anka create</code> would create a suspended VM. Starting in this version, VMs are stopped.</li><li>GUI (Anka.app) VM creation will produce a VM without macOS set up.</li><li><code>anka --machine-readable registry list</code> has a key name change from <code>id</code> to <code>uuid</code></li><li><code>modify set</code> commands will continue to work, but not show in the <code>anka modify --help</code> output.</li><li>Only macOS 10.15 and above are supported by Veertu. If you're using an older version, do not upgrade addons to 3.x and they should continue to function.</li></ul>{{< /rawhtml >}} |

Existing VM macOS Version | Target Version | Recommendation
--- | --- | ---
<= Big Sur | Monterey | You will need to change your VM template: `anka modify {vmName} set hard-drive -c sata`, `anka modify {vmName} set network-card -c virtio-net`
<= Big Sur | Ventura | You will need to change your VM template: `anka modify {vmName} set hard-drive -c sata`, `anka modify {vmName} set network-card -c virtio-net`, and `anka modify {vmName} set custom-variable hw.x2apic 1`
Monterey | Ventura | You will need to change your VM template: `anka modify {vmName} set custom-variable hw.x2apic 1`
<= Ventura | Ventura | (Intel only + 3.3.x) You will need to resuspend your Templates and push them to the registry.

### Upgrade Procedure

#### [1. Download and install the latest version]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})

{{< hint info >}}
Upgrading the Build Cloud too? Check out our [upgrade procedure for the Anka Build Cloud Controller & Registry]({{< relref "anka-build-cloud/upgrading.md" >}})
{{< /hint >}}

{{< hint warning >}}
We do not follow strict [semantic versioning](https://semver.org/); minor and major version increases can have significant changes
{{< /hint >}}

{{< hint warning >}}
Upgrading Anka Virtualization software while VMs are running **is NOT safe.**
{{< /hint >}}

{{< hint warning >}}
If you have the node joined to your Anka Build Cloud, the agent will restart itself in order to ensure it loads necessary components to support the new Anka version. You will see a message like `Version change detected! Stopping runner` in the logs. **Do not interrupt this processes as it is expected. Nodes may go offline temporarily.**
{{< /hint >}}

#### 2. Upgrade VM addons

If your existing version is noted in the [Pre-Upgrade Considerations]({{< relref "anka-virtualization-cli/upgrading.md#pre-upgrade-considerations" >}}) as requiring an addons upgrade, or, if you're going between major/minor versions of Anka:

   1. Upgrade the guest addons inside existing VM Templates `anka start -u`.
   2. Push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <newTag>`
