---
date: 2019-12-12T00:00:00-00:00
title: "Advanced Security Features"
linkTitle: "Advanced Security Features"
weight: 9
description: >
  Methods to setup authorization within your Controller & Registry.
---

> Advanced security features are currently only available for our Enterprise Tier licenses.

Our Enterprise Tiers include features to encrypt VMs at REST and even control VM properties like networking, copy/paste, etc through the VM policy ([Anka Secure](https://veertu.com/anka-secure/)).

There are three different authentication methods available:

**Method** | **Details**
--- | ---
[Root Token Authentication]({{< relref "./root-token-authentication.md" >}}) | Superuser access to Anka Controller dashboard.
[Certificate Authentication]({{< relref "./certificate-authentication.md" >}}) | Certificate based access to the Controller and Registry. API requests, Node joining, and [`anka registry`]({{< relref "docs/Anka Build Cloud/Virtualization CLI/command-reference.md#registry" >}}) commands all require a valid certificate.
[OpenId (SSO) Authentication]({{< relref "./openid-authentication.md" >}}) | Similar to Certificate authentication, but using your existing OpenID certified user directory.

> Authentication can be enabled in your [Controller configuration]({{< relref "docs/Anka Build Cloud/Controller and Registry/configuration-reference.md#authentication-and-authorization" >}}).
