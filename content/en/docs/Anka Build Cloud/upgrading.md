---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 9
description: How to upgrade the Anka Build Cloud
---

> We follow [semantic versioning](https://semver.org/); minor and major version increases can have significant changes.

## Upgrade Procedure

> **If Downgrading, be sure to follow step 5 to downgrade the agent version on each node**

When upgrading the entire Anka Build Cloud Software, execute these steps in the following sequence:

1) Run `sudo ankacluster disjoin` on your nodes. Then, wait for all active VMs/CI jobs to finish running.

2) [Install the latest Anka Build Virtualization CLI]({{< ref "docs/Getting Started/installing-the-anka-virtualization-package.md" >}})

3) _(only needed if noted in the [CLI notes matrix]({{< relref "docs/Anka Virtualization/upgrading.md#anka-build-virtualization-cli-upgrade-note-matrix" >}}))_ Upgrade the guest addons inside existing VM templates with `anka start -u`, then push the newly upgraded VM templates to registry with `anka registry push {vmNameOrUUID} --tag <tag>`

4) Go to your Controller & Registry server:

    - Docker: Make a backup of your `docker-compose.yml`! [Download and extract the latest package]({{< relref "docs/Anka Build Cloud/setup-on-linux-with-docker.md#step-2-install-the-anka-build-cloud-controller--registry" >}}), then either configure the values in the `docker-compose.yml` or copy your previous `docker-compose.yml` in and then run `docker-compose up --build --force-recreate --remove-orphans -d`
    - Native macOS package: Make a backup of your `/usr/local/bin/anka-controllerd`! Run `sudo anka-controller stop`, install the new .pkg (see the [MacOS Guide]({{< relref "docs/Anka Build Cloud/setup-on-macos.md" >}}), and then run `sudo anka-controller start`.
    
5) Run `curl -O http://anka.controller:80/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on your nodes to pull the latest Anka Agent binary and ensure proper communication between the CLI and the Controller API.

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