---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 6
description: How to upgrade the Anka Build Cloud
---

> We follow [semantic versioning](https://semver.org/); minor and major version increases can have significant changes.

## Upgrade Procedure

When upgrading the entire Anka Build Cloud Software, execute these steps in the following sequence:

1) Run `sudo ankacluster disjoin` on your nodes

2) [Install the latest Anka Build Virtualization CLI]({{< relref "docs/Anka Build Cloud/Virtualization CLI/installation.md" >}})

3) _(only needed if noted in the [CLI notes matrix]({{< relref "docs/Anka Build Cloud/Virtualization CLI/upgrading.md#anka-build-virtualization-cli-upgrade-note-matrix" >}}))_ Upgrade the guest addons inside existing VM templates with `anka start -u`, then push the newly upgraded VM templates to registry with `anka registry push {template} --tag <tag>`

5) Go to your Controller & Registry and upgrade to the latest version: [ [Linux Guide]({{< relref "docs/Anka Build Cloud/Controller and Registry/Setup/Linux/_index.md" >}}) OR [MacOS Guide]({{< relref "docs/Anka Build Cloud/Controller and Registry/Setup/Mac OS/_index.md" >}}) ]

## Anka Build Cloud upgrade note matrix

Existing Version | Target Version | Recommendation
--- | --- | ---
1.2.1 | 1.5.2 | Upgrade Anka CLI package on all the hosts to atleast 2.1.X
1.2.1 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.2.1 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.2.1 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Recommendation
--- | --- | ---
1.3.0 | 1.5.2 | Upgrade Anka CLI package on all the hosts to atleast 2.1.X
1.3.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.3.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.3.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Recommendation
--- | --- | ---
1.4.0 | 1.5.2 | No need to upgrade Anka CLI package on hosts
1.4.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.4.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2 but not necessary
1.4.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0 1.22.2 but not necessary

Existing Version | Target Version | Recommendation
--- | --- | ---
1.5.0 | 1.5.2 | No need to upgrade Anka CLI package on hosts
1.5.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.5.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2, but not necessary
1.5.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0, but not necessary