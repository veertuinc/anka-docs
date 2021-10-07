---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 10
description: How to upgrade the Anka Build Cloud
---

> We follow [semantic versioning](https://semver.org/); minor and major version increases can have significant changes

> These steps also apply to downgrading

> It is generally safe to upgrade the controller while VMs are running and nodes are joined. However, we do recommend temporarily pausing CI/CD jobs or assigning to agents and letting the currently running jobs drain before moving forward if you want to be extra careful

> **If upgrade the Anka Virtualization package:**
>
>   1. Block CI/CD jobs from starting or being assigned to agents
>   2. Wait for all CI/CD jobs to complete on your nodes
>   3. Run `sudo ankacluster disjoin` on each node
>
> You can then safely [install the latest Anka Build Virtualization CLI]({{< ref "docs/Getting Started/installing-the-anka-virtualization-package.md" >}})
>
> If your existing Anka Virtualization version is noted in the [Anka Virtualization Upgrade Matrix]({{< relref "docs/Anka Virtualization/upgrading.md#pre-upgrade-considerations" >}}):
>
>   1. Upgrade the guest addons inside existing VM templates with `anka start -u`
>   2. Push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <tag>`

> Before upgrading, check if your current version is noted in the [Pre-Upgrade Considerations]({{< relref "docs/Anka Build Cloud/upgrading.md#pre-upgrade-considerations" >}}) and adjust your upgrade plan accordingly.

> **Known issues we're working on fixes for (in versions >= 1.18.0):**
> Please note that there is a temporary workaround required for a bug that started in versions after 1.18.0 of the Controller/Registry agent which runs on your nodes. All versions of the agent, when noticing that the version of itself does not match the version of the controller, will perform a self-upgrade and restart. The restart seems to be problematic on some setups and leaves a zombie anka_agent process and and Offline status in the controller UI. To work around the bug when upgrading your controller/registry, you'll need to change the existing steps to include:
> - Disjoining all of the nodes first
> - Do the controller/registry upgrade
> - Run `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on each node to download the new version
> - And finally join each node back
> 
> We will be fixing the bug in our next release. Thanks for your understanding and we are sorry for the inconvenience this causes.

1. Go to your Controller & Registry server:

    - Docker:
        1. Make a backup of your `docker-compose.yml`.
        2. [Download and extract the latest package]({{< relref "docs/Anka Build Cloud/setup-on-linux-with-docker.md#step-2-install-the-anka-build-cloud-controller--registry" >}}).
        3. Configure the values in the `docker-compose.yml` or copy your previous `docker-compose.yml` to the new directory.
        4. Run `docker-compose build` to prepare the new docker tag.
        5. Run `docker-compose down` to take down the older version.
        6. Run `docker-compose up -d` in the newer version directory.
    - Native macOS package:
        1. Make a backup of your `/usr/local/bin/anka-controllerd`.
        2. Install the new .pkg (see the [MacOS Guide]({{< relref "docs/Anka Build Cloud/setup-on-macos.md" >}})).
        3. Run `sudo anka-controller restart`.

2. It's a good idea to [force the proper agent version to install across all of your nodes]({{< relref "docs/Anka Build Cloud/working-with-controller-and-API.md#force-node-agent-update" >}}). If you want to do it manually, you can `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on each node individually.

## Pre-Upgrade Considerations

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