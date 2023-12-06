---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading (CLOUD)"
linkTitle: "Upgrading"
weight: 12
description: How to upgrade the Anka Build Cloud
---

### Before you begin

{{< hint info >}}
We follow [semantic versioning](https://semver.org/); minor and major version increases can have significant changes and should be carefully considered.
{{< /hint >}}

{{< hint info >}}
Before upgrading, check if your current version is noted in the [Pre-Upgrade Considerations]({{< relref "anka-build-cloud/upgrading.md#pre-upgrade-considerations" >}}) and adjust your upgrade plan accordingly.
{{< /hint >}}

{{< hint info >}}
The Controller and Anka Nodes communicate through an agent running separate from from the anka-virtualization-cli/CLI tool, but on the same machine. When you upgrade the Controller, the node agent notices that the agent and controller versions differ and will create a task for each Node. This task will trigger the agent to perform a self-update and restart. While most situations this is seamless, we recommend checking the agent version post-upgrade on each Node with `ankacluster --version` to ensure it was upgraded properly. Nodes must be joined to the controller to receive the task to upgrade.

If necessary, you can [force the proper agent version task creation through the Controller API.]({{< relref "anka-build-cloud/working-with-controller-and-API.md#force-node-agent-update" >}}). Alternatively you can also disjoin the Nodes first, do the upgrade of the Controller, and then manually execute `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` (`AnkaAgentArm.pkg` if using Anka 3.0) on each node individually.
{{< /hint >}}

{{< hint info >}}
It is generally safe to upgrade the controller while VMs are running and nodes are joined. However, if you can, we do recommend temporarily pausing CI/CD jobs or assigning to agents and letting the currently running jobs drain before moving forward.
{{< /hint >}}

{{< hint info >}}
We recommend [snapshotting your etcd database]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md#etcd-snapshotting" >}}) regularly, but especially before an upgrade.
{{< /hint >}}

{{< hint warning >}}
If you are upgrading the host/node macOS version, please disjoin and join the node to the controller using the `ankacluster` command.
{{< /hint >}}

### Pre-upgrade Considerations

| Existing Version | Target Version | Recommendation |
| --- | --- | --- |
| < 1.36.1 | >= 1.36.1 | MacOS TLS users must set `ANKA_REGISTRY_LISTEN_ADDRESS` to a proper `IP:PORT`, vs the default `:PORT`, otherwise 127.0.0.1 will be used.
| < 1.36.0 | >= 1.36.0 | <ul><li>The registry used to (depending on configuration; enabled by default) start an additional un-secure server (on port 8085 by default) that is meant to be used by locally running Controller only. This un-secure server is no longer being ran by the Registry. We have removed this server and a few configurations `ANKA_INTERNAL_LISTEN_ADDR` & `ANKA_USE_HTTPS_INTERNAL`. **_Action required:_** Please ensure that any configuration using `ANKA_LOCAL_ANKA_REGISTRY` will instead point to the official 8089 port. Your TLS certificates may need to be updated to support a new address.</li><li>The Docker package has been rewritten to eliminate a lot of complexity. This includes removal of ENVs from Dockerfile/dockerhub images. You will want to review [our new docker-compose.yml](https://docs.veertu.com/anka/anka-build-cloud/configuration-reference/#docker-composeyml-docker) to see which ENVs are now being set there by default and ensure they are also included in your docker-compose.yml (or .env files). For example, the Registry needs `ANKA_LISTEN_ADDR: :8089` and `ANKA_BASE_PATH: /mnt/vol` set in the config/yml.</li></ul> |
| < 1.34.0 | 1.34.0 | The Controller will exclusively use the Root Token for communication and authentication with the Registry in 1.34.0. This means that both the Registry and Controller **must** have AUTH enabled as well as the same Root Token in their configs. Several ENVs like `ANKA_API_KEY_`, `ANKA_CLIENT_`, and `ANKA_OIDC_` (registry only) are no longer available and necessary. They can be removed from your configuration, but be sure to mirror the AUTH configuration for Root Token and other ENVS from the Controller. |
| < 1.32.0 | >= 1.33.0 | This release removed Implicit Flow OIDC support. Customers will need to review [the new Code/Explicit Flow and its requirements]({{< relref "whats-new/build-cloud-1.33.0/index.md#codeexplicit-flow-oidc-support" >}}). |
| < 1.21.0 | >= 1.21.0 | The docker package (.tar.gz) was redesigned and has lot of changes around the use of ENVs and method of binary execution. We recommend using the new https://docs.veertu.com/anka/anka-build-cloud/getting-started/setup-controller-and-registry/ guide, from scratch in your environment, if upgrading from a version prior to 1.21.0. |
| 1.18.0 | > 1.18.0 | Please note that there is a temporary workaround required for a bug that started in versions after 1.18.0 of the Controller/Registry agent which runs on your nodes. All versions of the agent, when noticing that the version of itself does not match the version of the controller, will perform a self-upgrade and restart. The restart seems to be problematic on some setups and leaves a zombie anka_agent process and and Offline status in the controller UI. To work around the bug when upgrading your controller/registry, you'll need to change the existing steps to include: <ul><li>Disjoining all of the nodes first</li><li>Do the controller/registry upgrade</li><li>Run `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` (`AnkaAgentArm.pkg` if using Anka 3.0) on each node to download the new version</li><li>And finally join each node back</li></ul>We will be fixing the bug soon. Thanks for your understanding and we are sorry for the inconvenience this causes.
| x.xx.x | 1.20.0 | _Minimum Registry version required for Controller - 1.19.0_
| < 1.24.0 | 1.24.0 | SHA1 certificates are no longer supported for TLS/HTTPS & Certificate Authentication.

### Upgrade Procedure

{{< hint warning >}}
**The following steps also apply to downgrading, though, you need to forcefully downgrade the cluster agent on each of your nodes (see step 7).**
{{< /hint >}}

{{< hint warning >}}
If you are not using our ETCD container, be sure that you upgrade ETCD to the version we require (see release notes) before trying to update the Controller and Registry.
{{< /hint >}}


#### Docker

  1. Make a backup of your `docker-compose.yml`.
  2. [Download and extract the latest package]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}).
  3. Configure the values in the `docker-compose.yml` or copy your previous `docker-compose.yml` to the new directory. You will need to manually copy ENVs you're using in `.env` files of older versions into the `docker-compose.yml` under `environment:`.
  4. Run `docker-compose build` in the new package directory to prepare the new docker tag.
  5. Run `docker-compose down` in the previous package directory to take down the older version.
  6. Run `docker-compose up -d` in the newer version directory to finally bring it up.
  7. (downgrading only) Disjoin any nodes which are connected, then run the following on each Anka Node: `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` (`AnkaAgentArm.pkg` if using Anka 3/ARM)

{{< hint info >}}
You can temporarily run `docker-compose up` which attached you immediately to the service logs so you can watch for errors. However, it will not stay running so once things are confirmed to be working, you'd need to `docker-compose down` and `docker-compose up -d` before logging out of the server.
{{< /hint >}}

{{< hint warning >}}
Post-upgrade of the controller you will need to wait for Anka Nodes to finish upgrading their agents. This can take a minute or two. Node statuses should say "Upgrading". Please be patient for this process.
{{< /hint >}}

#### Native macOS package

  1. Make a backup of your `/usr/local/bin/anka-controllerd` and `/usr/local/bin/anka-registryd`.
  2. Install the new packages (see the [MacOS Guide]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry-mac.md" >}})).
  3. Run `sudo anka-controller restart` and `sudo anka-registry restart`.
