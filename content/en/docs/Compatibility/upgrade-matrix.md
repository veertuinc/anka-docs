---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading Anka Software"
linkTitle: "Upgrading Anka Software"
weight: 3
description: >
  Anka Software Upgrade Matrix and Notes
---

> We follow [semantic versioning](https://semver.org/)

## Anka Build CLI upgrade matrix

Existing Version | Target Version | Recommendation
--- | --- | ---
1.4.3 | 2.x.x | Requires upgrade of all existing VM templates with `anka start -u` and push to the registry
2.x | 2.1.2 | Only requires upgrade of existing Catalina VM templates with `anka start -u` and push to the registry

## Anka Build Cloud upgrade matrix
Existing Version | Target Version | Recommendation
--- | --- | ---
1.0.14 | 1.5.2 | Upgrade Anka CLI package on all the hosts to atleast 2.1.X
1.0.14 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.0.14 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.0.14 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Recommendation
--- | --- | ---
1.2.0 | 1.5.2 | Upgrade Anka CLI package on all the hosts to atleast 2.1.X
1.2.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.2.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.2.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

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




