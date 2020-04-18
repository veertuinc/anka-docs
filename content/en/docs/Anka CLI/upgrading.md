---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 6
description: How to upgrade the Anka CLI
---

## Upgrade 

1) Install the new anka pkg. Follow the [install instructions]({{< relref "installation.md" >}})

2) Upgrade the guest addons inside the VM templates with `anka start -u`. Check the upgrade notes to see if this step is necessary.


## Upgrade When running as a part of an Anka Build cloud
When upgrading the entire Anka Build infrastructure (Controller/Registry and Nodes), execute the steps in the following sequence.

1) Run `sudo ankacluster disjoin`.

2) Install the new anka pkg. Follow the [install instructions]({{< relref "installation.md" >}})

3) Upgrade the guest addons inside the VM templates with `anka start -u`. Check the upgrade notes to see if this step is necessary.

4) Push the newly upgraded VM templates to registry with `anka registry push <template> --tag <tag>`.

5) Go to the Controller/Registry and upgrade.

6) On the mac nodes - run `sudo ankacluster join`.
