---
date: 2019-12-12T00:00:00-00:00
title: "Advanced Security Features (CLOUD)"
linkTitle: "Advanced Security Features"
weight: 8
description: >
  Methods to protect your Anka Build Cloud Controller & Registry.
---

{{< hint info >}}
Advanced security features are currently only available for our Enterprise Tier licenses.
{{< /hint >}}

{{< hint info >}}
You must have at least one node with a Enterprise or higher license joined to the Controller for these features to enable.
{{< /hint >}}

## Overview

We provide several authorization and authentication methods for the Anka Build Cloud. They are split into **Human** and **Programmatic Access**.

### Human Access

These methods are available to a human through a browser and the Controller UI.

- [Root Token]({{< relref "anka-build-cloud/Advanced Security Features/token-authentication.md#protecting-your-cloud-with-rta-root-token-auth" >}})
- [OIDC / SSO]({{< relref "anka-build-cloud/Advanced Security Features/openid-authentication.md" >}})

### Programmatic Access

These methods are available for users and scripts that need to use the Controller API, Registry API, or are using the Anka CLI through `ankacluster join --api-*` and `anka registry --api-*` options.

- [Root Token]({{< relref "anka-build-cloud/Advanced Security Features/token-authentication.md#protecting-your-cloud-with-rta-root-token-auth" >}})
- [UAK+TAP]({{< relref "anka-build-cloud/Advanced Security Features/token-authentication.md" >}})
- [MTLS / Certificates]({{< relref "anka-build-cloud/Advanced Security Features/certificate-authentication.md" >}})
