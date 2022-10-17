---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 5
description: How to upgrade the Anka Virtualization package
---


{{< hint info >}}
Upgrading the Build Cloud too? Check out our [upgrade procedure for the Anka Build Cloud Controller & Registry]({{< relref "Anka Build Cloud/upgrading.md" >}})
{{< /hint >}}

{{< hint warning >}}
We do not follow strict [semantic versioning](https://semver.org/); minor and major version increases can have significant changes
{{< /hint >}}

<!-- ### Pre-upgrade Considerations -->

<!-- Existing Version | Target Version | Recommendation
--- | --- | --- -->


### Upgrade Procedure

#### [1. Download and install the latest version]({{< relref "apple/Getting Started/installing-the-anka-virtualization-package.md" >}})

#### [2. Upgrade VM addons](#upgrade-vm-addons)

If your existing version is noted in the [Pre-Upgrade Considerations]({{< relref "intel/upgrading.md#pre-upgrade-considerations" >}}) as requiring an addons upgrade, or, if you're going between major/minor versions of Anka:

   1. Upgrade the guest addons inside existing VM templates with `anka start -vu`. This opens the VM desktop in a viewer window and allows you to manually launch and install the addons package.
   2. Push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <newTag>`
