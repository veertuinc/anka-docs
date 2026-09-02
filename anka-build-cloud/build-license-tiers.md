---
date: 2019-12-12T00:00:00-00:00
title: "Build License Tiers"
linkTitle: "Build License Tiers"
weight: 14
description: >
  All about the Anka Build Cloud license tiers
---

## Anka License Feature Differences

{{< include file="_partials/Licensing/feature-differences.md" >}}

{{< include file="_partials/Licensing/build-tiers.md" >}}

## Enterprise License Features

### Node Groups

{{< include file="_partials/anka-build-cloud/_node_groups.md" >}}

### Priority Scheduling

{{< include file="_partials/anka-build-cloud/_priority-scheduling.md" >}}

### [Controller authentication (Certificate, Root Token, and UAK/TAP)]({{< relref "anka-build-cloud/Advanced Security Features/_index.md" >}})

Authentication support includes:

- [Root Token]({{< relref "anka-build-cloud/Advanced Security Features/root-token-authentication.md" >}}) access to the Controller Dashboard
- [Certificate (mTLS) authentication]({{< relref "anka-build-cloud/Advanced Security Features/certificate-authentication.md" >}}) for Build Nodes, plugins, API clients, and Anka CLI access to the Registry
- [UAK/TAP]({{< relref "anka-build-cloud/Advanced Security Features/uak-tap-authentication.md" >}}) for programmatic API, CLI, and agent access

With an Enterprise license, each authenticated credential has full access to the system. Fine-grained Permission Groups and Resource Management require Enterprise Plus.

## Enterprise Plus License Features

### [Multi-user and group authorization with admin panel + OpenID/SSO support]({{< relref "anka-build-cloud/Advanced Security Features/openid-authentication.md" >}})

Enterprise Plus adds multi-user authentication and authorization for the Controller Dashboard and REST API. OpenID/SSO integrates human access through your identity provider. Map provider groups to Controller Permission Groups to control what each user can do.

### [Permission Groups and Resource Management]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authorization" >}})

Permission Groups limit which API actions a credential can perform. Resource Management further limits access to specific Nodes and Templates. Enable them with `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` / `ANKA_ENABLE_REGISTRY_AUTHORIZATION` and `ANKA_ENABLE_RESOURCE_MANAGEMENT`. See [Authorization]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authorization" >}}) for setup details.

UAK/TAP and certificate credentials behave differently under Enterprise Plus: user tokens and cert subjects must use Permission Groups. The root token keeps full access.

### Event logging and automated pushing

{{< include file="_partials/anka-build-cloud/_event-logging-endpoint-pushing.md" >}}
