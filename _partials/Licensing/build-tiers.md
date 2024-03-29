

## Anka Build License Tier Datasheet

**Feature** | **Basic** | **Enterprise** | **Enterprise Plus**
--- | --- | --- |  ---
Cloud Controller with REST APIs | Yes(Single instance of Anka controller included) | Yes(Single instance of Anka controller included) | Yes
[GitHub Action](https://github.com/marketplace/actions/anka-vm-github-action) | Yes | Yes | Yes
[Jenkins Plugin](https://plugins.jenkins.io/anka-build/) | Yes | Yes | Yes
[TeamCity Plugin](https://plugins.jetbrains.com/plugin/10733-anka-build-cloud) | Yes | Yes | Yes
[GitLab Runner with custom executor](https://github.com/veertuinc/gitlab-runner) | Yes | Yes | Yes
[BuildKite Plugin](https://github.com/veertuinc/anka-buildkite-plugin) | Yes | Yes | Yes
HA for Controller configuration setup | Yes (Additional controller/registry instances needed) | Yes | Yes
[USB Device control through Controller API]({{< relref "anka-build-cloud/working-with-controller-and-api.md#usb-device-control-controller-api" >}}) |    | Yes | Yes
[Priority scheduling of VMs through controller]({{< relref "anka-build-cloud/working-with-controller-and-api.md#priority-scheduling" >}}) |    | Yes | Yes
[Clustering (Grouping) of Nodes]({{< relref "anka-build-cloud/working-with-controller-and-api.md#node-groups" >}}) |    | Yes | Yes 
[Basic controller authentication (Certificate & Root Superuser Token)]({{< relref "anka-build-cloud/Advanced Security Features/_index.md" >}}) |    | Yes | Yes
[Multi-user & group authorization with admin panel + OpenID/SSO support]({{< relref "anka-build-cloud/Advanced Security Features/openid-authentication.md" >}}) |    |    | Yes
[Controller API event logging]({{< relref "anka-build-cloud/working-with-controller-and-API.md#event-logging-and-automated-pushing" >}}) |    |    | Yes