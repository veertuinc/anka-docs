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

### Event logging and automated pushing

{{< include file="_partials/anka-build-cloud/_event-logging-endpoint-pushing.md" >}}

### [Basic controller authentication (Certificate & Root Superuser Token)]({{< relref "anka-build-cloud/Advanced Security Features/_index.md" >}})

Authentication support includes Root token authentication access to the Controller Dashboard and certificate authentication for the following clients: Build nodes, plugins, API access, anka command line access to the registry.

## Enterprise Plus License Features

### [Multi-user & group authorization with admin panel + OpenID/SSO support]({{< relref "anka-build-cloud/Advanced Security Features/openid-authentication.md" >}})

Multi-user access authentication and authorization based access to Controller portal dashboard and REST API operations is provided through OpenID/SSO based integration