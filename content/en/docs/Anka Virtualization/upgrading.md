---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 9
description: How to upgrade the Anka Virtualization package
---

> We do not follow strict [semantic versioning](https://semver.org/); minor and major version increases can have significant changes

> Upgrading Anka Virtualization software while VMs are running **is safe**

> **Upgrading macOS VM inside Anka VM**
Anka supports `softwareupdate` and also the System Preferences method of upgrading.

### Upgrade Procedure (without Anka Build Cloud Controller & Registry)

1. [Download and install the latest version]({{< relref "docs/Getting Started/installing-the-anka-virtualization-package.md" >}})

> If your existing version is noted in the [Pre-Upgrade Considerations]({{< relref "docs/Anka Virtualization/upgrading.md#pre-upgrade-considerations" >}}):
>
>   1. Upgrade the guest addons inside existing VM templates with `anka start -u`
>   2. Push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <tag>`

### [Upgrade Procedure (with Anka Build Cloud Controller & Registry)]({{< relref "docs/Anka Build Cloud/upgrading.md" >}})
### Pre-Upgrade Considerations

Existing Version | Target Version | Recommendation
--- | --- | ---
1.4.3 | 2.x.x | Requires upgrade of all existing VM templates with `anka start --update` and push to the registry
2.x | 2.1.2 | Only requires upgrade of existing Catalina VM templates with `anka start --update` and push to the registry

### Upgrading macOS inside of a running VM

This scenario is possible, however, it's recommended that you upgrade Anka addons before upgrading macOS.