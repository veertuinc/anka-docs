---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 10
description: How to upgrade the Anka Build Cloud
---

> We follow [semantic versioning](https://semver.org/); minor and major version increases can have significant changes.

> **If upgrading the Anka Virtualization package:**
>
>   1. Block CI/CD jobs from starting or being assigned to agents
>   2. Wait for all CI/CD jobs to complete on your nodes
>   3. Run `sudo ankacluster disjoin` on each node
>
> You can then safely [install the latest Anka Build Virtualization CLI]({{< relref "arm/Getting Started/installing-the-anka-virtualization-package.md" >}})

### Upgrade Procedure

> Before upgrading, check if your current version is noted in the [Pre-Upgrade Considerations]({{< relref "arm/Anka Build Cloud/upgrading.md#pre-upgrade-considerations" >}}) and adjust your upgrade plan accordingly.

> It is generally safe to upgrade the controller while VMs are running and nodes are joined. However, we do recommend temporarily pausing CI/CD jobs or assigning to agents and letting the currently running jobs drain before moving forward if you want to be extra careful.

The Controller and Anka Nodes communicate through an agent running separate from from the Anka Virtualization/CLI tool. When you upgrade the Controller, it will notice that joined Nodes differ in their version and will create a task that each Node will pick up. This task will trigger the agent to perform a self-update and restart. While most situations this is seamless, we recommend checking the agent version on each Node with `ankacluster --version` to ensure it was upgraded properly. Nodes must be joined to the controller to receive the task to upgrade.

> If necessary, you can [force the proper agent version task creation through the Controller API.]({{< relref "arm/Anka Build Cloud/working-with-controller-and-API.md#force-node-agent-update" >}}). 
>
> Alternatively you can also disjoin the Nodes first, do the upgrade of the Controller, and then manually execute `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on each node individually.

#### Go to your Controller & Registry server:

> The following steps also apply to downgrading, though, you need to forcefully downgrade the cluster agent on each of your nodes.

- ***Docker:***
  1. Make a backup of your `docker-compose.yml`.
  2. [Download and extract the latest package]({{< relref "arm/Anka Build Cloud/setup-on-linux-with-docker.md#step-2-install-the-anka-build-cloud-controller--registry" >}}).
  3. Configure the values in the `docker-compose.yml` or copy your previous `docker-compose.yml` to the new directory.
  4. Run `docker-compose build` to prepare the new docker tag.
  5. Run `docker-compose down` to take down the older version.
  6. Run `docker-compose up -d` in the newer version directory.

- ***Native macOS package:***
  1. Make a backup of your `/usr/local/bin/anka-controllerd`.
  2. Install the new .pkg (see the [MacOS Guide]({{< relref "arm/Anka Build Cloud/setup-on-macos.md" >}})).
  3. Run `sudo anka-controller restart`.

