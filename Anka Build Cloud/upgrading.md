---
date: 2019-12-12T00:00:00-00:00
title: "Upgrading"
linkTitle: "Upgrading"
weight: 12
description: How to upgrade the Anka Build Cloud
---

### Before you begin

{{< hint info >}}
We follow [semantic versioning](https://semver.org/); minor and major version increases can have significant changes and should be carefully considered.
{{< /hint >}}

{{< hint info >}}
Before upgrading, check if your current version is noted in the [Pre-Upgrade Considerations]({{< relref "Anka Build Cloud/upgrading.md#pre-upgrade-considerations" >}}) and adjust your upgrade plan accordingly.
{{< /hint >}}

{{< hint info >}}
The Controller and Anka Nodes communicate through an agent running separate from from the anka-virtualization-cli/CLI tool, but on the same machine. When you upgrade the Controller, the node agent notices that the agent and controller versions differ and will create a task for each Node. This task will trigger the agent to perform a self-update and restart. While most situations this is seamless, we recommend checking the agent version post-upgrade on each Node with `ankacluster --version` to ensure it was upgraded properly. Nodes must be joined to the controller to receive the task to upgrade.

If necessary, you can [force the proper agent version task creation through the Controller API.]({{< relref "Anka Build Cloud/working-with-controller-and-API.md#force-node-agent-update" >}}). Alternatively you can also disjoin the Nodes first, do the upgrade of the Controller, and then manually execute `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` (`AnkaAgentArm.pkg` if using Anka 3.0) on each node individually.
{{< /hint >}}

{{< hint info >}}
It is generally safe to upgrade the controller while VMs are running and nodes are joined. However, if you can, we do recommend temporarily pausing CI/CD jobs or assigning to agents and letting the currently running jobs drain before moving forward.
{{< /hint >}}

{{< hint info >}}
We recommend [snapshotting your etcd database]({{< relref "Anka Build Cloud/getting-started/setup-controller-and-registry.md#etcd-snapshotting" >}}) regularly, but especially before an upgrade.
{{< /hint >}}

{{< hint warning >}}
If you are upgrading the host/node macOS version, please disjoin and join the node to the controller using the `ankacluster` command.
{{< /hint >}}

### Pre-upgrade Considerations

| Existing Version | Target Version | Recommendation |
| --- | --- | --- |
| < 1.21.0 | >= 1.21.0 | The docker package (.tar.gz) was redesigned and has lot of changes around the use of ENVs and method of binary execution. We recommend using the new https://docs.veertu.com/anka/anka-build-cloud/getting-started/setup-controller-and-registry/ guide, from scratch in your environment, if upgrading from a version prior to 1.21.0. |
| 1.18.0 | > 1.18.0 | Please note that there is a temporary workaround required for a bug that started in versions after 1.18.0 of the Controller/Registry agent which runs on your nodes. All versions of the agent, when noticing that the version of itself does not match the version of the controller, will perform a self-upgrade and restart. The restart seems to be problematic on some setups and leaves a zombie anka_agent process and and Offline status in the controller UI. To work around the bug when upgrading your controller/registry, you'll need to change the existing steps to include: <ul><li>Disjoining all of the nodes first</li><li>Do the controller/registry upgrade</li><li>Run `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` (`AnkaAgentArm.pkg` if using Anka 3.0) on each node to download the new version</li><li>And finally join each node back</li></ul>We will be fixing the bug soon. Thanks for your understanding and we are sorry for the inconvenience this causes.
| x.xx.x | 1.20.0 | _Minimum Registry version required for Controller - 1.19.0_
| < 1.24.0 | 1.24.0 | SHA1 certificates are no longer supported for TLS/HTTPS & Certificate Authentication. 

### Upgrade Procedure

{{< hint warning >}}
**The following steps also apply to downgrading, though, you need to forcefully downgrade the cluster agent on each of your nodes.**
{{< /hint >}}

#### Docker

  1. Make a backup of your `docker-compose.yml`.
  2. [Download and extract the latest package]({{< relref "Anka Build Cloud/getting-started/setup-controller-and-registry.md#step-2-install-the-anka-build-cloud-controller--registry" >}}).
  3. Configure the values in the `docker-compose.yml` or copy your previous `docker-compose.yml` to the new directory and also any .env files you have under the various service directories.
  4. Run `docker-compose build` in the new package directory to prepare the new docker tag.
  5. Run `docker-compose down` in the previous package directory to take down the older version.
  6. Run `docker-compose up -d` in the newer version directory to finally bring it up.

{{< hint info >}}
You can temporarily run `docker-compose up` which attached you immediately to the service logs so you can watch for errors. However, it will not stay running so once things are confirmed to be working, you'd need to `docker-compose down` and `docker-compose up -d` before logging out of the server.
{{< /hint >}}

{{< hint warning >}}
Post-upgrade of the controller you will need to wait for Anka Nodes to finish upgrading their agents. This can take a minute or two. Node statuses should say "Upgrading". Please be patient for this process.
{{< /hint >}}

#### Native macOS package

  1. Make a backup of your `/usr/local/bin/anka-controllerd`.
  2. Install the new .pkg (see the [MacOS Guide]({{< relref "Anka Build Cloud/getting-started/setup-controller-and-registry.md#macos" >}})).
  3. Run `sudo anka-controller restart`.
