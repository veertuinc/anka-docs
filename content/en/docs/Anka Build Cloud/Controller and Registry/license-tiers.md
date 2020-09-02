---
date: 2019-12-12T00:00:00-00:00
title: "License Tiers"
linkTitle: "License Tiers"
weight: 12
description: >
  All about the Anka Build Cloud license tiers
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
[USB Device control through the CLI]({{< relref "docs/Anka Virtualization/working-with-usb-devices.md" >}}) |  Yes  | Yes | Yes
[USB Device control through Controller API]({{< relref "#usb-device-control-controller-api" >}}) |    | Yes | Yes
[Priority scheduling of VMs through controller]({{< relref "#priority-scheduling" >}}) |    | Yes | Yes
[Clustering (Grouping) of Nodes]({{< relref "#node-groups" >}}) |    | Yes | Yes 
[Basic controller authentication (Certificate & Root Superuser Token)]({{< relref "docs/Anka Build Cloud/Controller and Registry/Advanced Security Features/_index.md" >}}) |    | Yes | Yes
[Multi-user & group authorization with admin panel + OpenID/SSO support]({{< relref "docs/Anka Build Cloud/Controller and Registry/Advanced Security Features/openid-authentication.md" >}}) |    |    | Yes
Controller API activity logging |    |    | Yes
VM full disk encryption for Build VMs |    |    | Yes
Control VM runtime (Networking, Access to host) and functional properties with policies |    |    | Yes 

## Enterprise License Features

### Node Groups

{{< include file="shared/content/en/docs/Anka Build Cloud/Controller and Registry/partials/_node_groups.md" >}}

### Priority Scheduling

{{< include file="shared/content/en/docs/Anka Build Cloud/Controller and Registry/partials/_priority-scheduling.md" >}}

### USB Device Control (Controller API)

{{< include file="shared/content/en/docs/Anka Build Cloud/Controller and Registry/partials/_usb-device-control-api.md" >}}

### [Basic controller authentication (Certificate & Root Superuser Token)]({{< relref "docs/Anka Build Cloud/Controller and Registry/Advanced Security Features/_index.md" >}})

Authentication support includes Root token authentication access to the Controller Dashboard and certificate authentication for the following clients: Build nodes, plugins, API access, anka command line access to the registry.

## Enterprise Plus License Features

### [Multi-user & group authorization with admin panel + OpenID/SSO support]({{< relref "docs/Anka Build Cloud/Controller and Registry/Advanced Security Features/openid-authentication.md" >}})

Multi-user access authentication and authorization based access to Controller portal dashboard and REST API operations is provided through OpenID/SSO based integration.

### VM encryption of Build VMs at REST

Encrypt the build VM template at the time of VM creation and store it in the Anka Registry in an encrypted state. When this VM is used to build on the Anka Build nodes, it will be decrypted.

### VM Control through Policy

Manage the Build VMs access to local and remote resources through policies. This includes the ability to control VM access to host shared folders, USB, disk access, networking, and external networking.
