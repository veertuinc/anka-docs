---
date: 2019-12-12T00:00:00-00:00
title: "Advanced Security Features"
linkTitle: "Advanced Security Features"
weight: 8
description: >
  Methods to setup authorization within your Controller & Registry.
---

{{< hint info >}}
Advanced security features are currently only available for our Enterprise Tier licenses.
{{< /hint >}}

{{< hint info >}}
You must have at least one node with a Enterprise or higher license joined to the Controller for these features to work.
{{< /hint >}}

{{< hint info >}}
Authentication can be enabled in your [Controller configuration]({{< relref "Anka Build Cloud/configuration-reference.md#authentication-and-authorization" >}}).
{{< /hint >}}

There are three different authentication methods available:

**Method** | **Details**
--- | ---
[Token Authentication]({{< relref "./token-authentication.md" >}}) | Token based auth tokens & Root token access to Anka Controller dashboard (and API).
[Certificate Authentication]({{< relref "./certificate-authentication.md" >}}) | Certificate based access to the Controller and Registry. API requests, Node joining, and [`anka registry`]({{< relref "intel/command-line-reference.md#registry" >}}) commands all require a valid certificate.
[OpenId (SSO) Authentication]({{< relref "./openid-authentication.md" >}}) | Similar to Certificate authentication, but using your existing OpenID certified user directory.
