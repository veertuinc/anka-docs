---
---

## Anka Build License Tier Datasheet

**Feature** | **Basic** | **Enterprise** | **Enterprise Plus**
--- | --- | --- |  ---
Core based licensing | Yes | Yes | Yes
Cloud Controller with REST APIs | Yes(Single instance of Anka controller included) | Yes(Single instance of Anka controller included) | Yes
Central Registry | Yes(Single instance Anka Registry included) | Yes | Yes
[GitHub Action](https://github.com/marketplace/actions/anka-vm-github-action) | Yes | Yes | Yes
[Jenkins Plugin](https://plugins.jenkins.io/anka-build/) | Yes | Yes | Yes
[TeamCity Plugin](https://plugins.jetbrains.com/plugin/10733-anka-build-cloud) | Yes | Yes | Yes
[GitLab Runner with custom executor](https://github.com/veertuinc/gitlab-runner) | Yes | Yes | Yes
[BuildKite Plugin](https://github.com/veertuinc/anka-buildkite-plugin) | Yes | Yes | Yes
HA for Controller configuration setup | Yes (Additional controller/registry instances needed) | Yes | Yes
[USB Device control through the CLI (Anka 2 Only)]({{< relref "intel/working-with-usb-devices.md" >}}) |  Yes  | Yes | Yes
[USB Device control through Controller API (Anka 2 Only)]({{< relref "#usb-device-control-controller-api" >}}) |    | Yes | Yes
[Priority scheduling of VMs through controller]({{< relref "#priority-scheduling" >}}) |    | Yes | Yes
[Clustering (Grouping) of Nodes]({{< relref "#node-groups" >}}) |    | Yes | Yes 
[Basic controller authentication (Certificate & Root Superuser Token)]({{< relref "Anka Build Cloud/Advanced Security Features/_index.md" >}}) |    | Yes | Yes
[Multi-user & group authorization with admin panel + OpenID/SSO support]({{< relref "Anka Build Cloud/Advanced Security Features/openid-authentication.md" >}}) |    |    | Yes
Controller API activity logging |    |    | Yes