


---
date: 2019-12-12T00:00:00-00:00
title: "Upgrade notes"
linkTitle: "Upgrade notes"
weight: 3
description: >
  Some points to note when upgrading Anka
---

* Upgrade to Controller version 1.4.0 doesn't require upgrade of jenkins plugins to version 1.21.0.
* Upgrade to Controller version 1.4.0 requires **Anka package version 2.1.X**.
* Upgrade to Anka package version **2.1.2 from version 2.x** requires upgrade of VM templates `anka start -u`only for Catalina VMs.
* Upgrade to Anka package version **2.1.1 from version 2.x DOES NOT** require upgrade of VM templates. No need for `anka start -u`.
* Upgrade to Anka package version **2.x from version 1.4 DOES** require upgrade of VM templates using `anka start -u` and repush to the registry.
* Upgrade to Controller version 1.3 doesn't require upgrade of jenkins plugins to version 1.20.
* Upgrade to Controller version 1.3 requires **Anka package version 2.1**.
* Upgrade to Controller version 1.3 also **requires upgrade of Registry to version 1.3**.
* Upgrade to Controller version 1.2.1 from 1.2.0 can also be done by just **upgrading the  Registry to version 1.2.1**.
* Upgrade to Controller version 1.2.1 also **requires upgrade of Registry to version 1.2.1**.
* Upgrade to Controller version 1.2.1 also **requires upgrade of Anka Binary on the nodes to version 2.1**.
* Upgrade to Controller version 1.2.1 **DOES NOT** require upgrade of Jenkins Anka Build Plugin to the latest version 1.19.1.
* Upgrade to Controller version 1.2.1 **DOES NOT** require upgrade of Jenkins Anka Slave Builder Plugin to the latest version 1.5.
* Upgrade to Controller version 1.2.1 **DOES NOT** require upgrade of Teamcity Anka Slave Builder Plugin to the latest version 1.6.
* Upgrade to Anka package version **2.1 from version 2.x DOES NOT** require upgrade of VM templates. No need for `anka start -u`.
* We will publish **frequent releases of Controller package with bug fixes and new managibility features. These releases will be backward compatible with Anka package version 2.1 onwards and will not require you to upgrade Anka package on the nodes**.
* We will publish **major releases of Anka a couple of times a year. Minor updates to Anka package will be published for any critical bug fix**.


