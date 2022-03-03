---
date: 2019-12-12T00:00:00-00:00
title: "Build License Tiers"
linkTitle: "Build License Tiers"
weight: 13
description: >
  All about the Anka Build Cloud license tiers
---

## Anka License Feature Differences

{{< include file="_partials/Licensing/feature-differences.md" >}}

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
[USB Device control through the CLI (Anka 2 Only)]({{< archRelRef "Anka Virtualization/working-with-usb-devices.md" >}}) |  Yes  | Yes | Yes
[USB Device control through Controller API (Anka 2 Only)]({{< archRelRef "Anka Build Cloud/working-with-controller-and-API.md#usb-device-control-controller-api" >}}) |    | Yes | Yes
[Priority scheduling of VMs through controller]({{< relref "#priority-scheduling" >}}) |    | Yes | Yes
[Clustering (Grouping) of Nodes]({{< archRelRef "Anka Build Cloud/working-with-controller-and-API.md#node-groups" >}}) |    | Yes | Yes
[Basic controller authentication (Certificate & Root Superuser Token)]({{< archRelRef "Anka Build Cloud/Advanced Security Features/_index.md" >}}) |    | Yes | Yes
[Multi-user & group authorization with admin panel + OpenID/SSO support]({{< archRelRef "Anka Build Cloud/Advanced Security Features/openid-authentication.md" >}}) |    |    | Yes
Controller API activity logging |    |    | Yes

## Enterprise License Features

### Node Groups

{{< include file="_partials/Anka Build Cloud/_node_groups.md" >}}

### Priority Scheduling

{{< include file="_partials/Anka Build Cloud/_priority-scheduling.md" >}}

### Event logging and automated pushing

{{< include file="_partials/Anka Build Cloud/_event-logging-endpoint-pushing.md" >}}

### [Basic controller authentication (Certificate & Root Superuser Token)]({{< archRelRef "Anka Build Cloud/Advanced Security Features/_index.md" >}})

Authentication support includes Root token authentication access to the Controller Dashboard and certificate authentication for the following clients: Build nodes, plugins, API access, anka command line access to the registry.

## Enterprise Plus License Features

### [Multi-user & group authorization with admin panel + OpenID/SSO support]({{< archRelRef "Anka Build Cloud/Advanced Security Features/openid-authentication.md" >}})

Multi-user access authentication and authorization based access to Controller portal dashboard and REST API operations is provided through OpenID/SSO based integration
