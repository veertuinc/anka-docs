


---
date: 2019-12-12T00:00:00-00:00
title: "Upgrade notes"
linkTitle: "Upgrade notes"
weight: 3
description: >
  Anka Upgrade Matrix
---
* We will publish **frequent releases of Controller package with bug fixes and new managibility features. These releases will be backward compatible with Anka package version 2.1.1 or later and will not require you to upgrade Anka package on the nodes**.
* We will publish **major releases of Anka only a couple of times a year. Minor updates to Anka package will be published for any critical bug fix**.

**Anka Package Upgrade matrix**
Existing Version | Target Version | Should do this
--- | --- | ---
1.4.3 | 2.x.x | requires upgrade of all existing VM templates with `anka start -u` and push to the registry
2.x | 2.1.2 | Only requires upgrade of existing Catalina VM templates with `anka start -u` and push to the registry

**Controller Upgrade matrix**
Existing Version | Target Version | Should do this
--- | --- | ---
1.0.14 | 1.5.2 | Upgrade Anka package on all the hosts to atleast 2.1.X
1.0.14 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.0.14 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.0.14 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Should do this
--- | --- | ---
1.2.0 | 1.5.2 | Upgrade Anka package on all the hosts to atleast 2.1.X
1.2.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.2.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.2.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Should do this
--- | --- | ---
1.2.1 | 1.5.2 | Upgrade Anka package on all the hosts to atleast 2.1.X
1.2.1 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.2.1 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.2.1 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Should do this
--- | --- | ---
1.3.0 | 1.5.2 | Upgrade Anka package on all the hosts to atleast 2.1.X
1.3.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.3.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2
1.3.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0

Existing Version | Target Version | Should do this
--- | --- | ---
1.4.0 | 1.5.2 | No need to upgrade Anka package on hosts
1.4.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.4.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2 but not necessary
1.4.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0 1.22.2 but not necessary

Existing Version | Target Version | Should do this
--- | --- | ---
1.5.0 | 1.5.2 | No need to upgrade Anka package on hosts
1.5.0 | 1.5.2 | Recommended to upgrade Anka Registry to version 1.5.2, if you want to use cache builder
1.5.0 | 1.5.2 | Recommended to upgrade Anka Jenkins Plugin to version 1.22.2, but not necessary
1.5.0 | 1.5.2 | Recommended to upgrade Anka TeamCity Plugin to version 1.7.0, but not necessary




