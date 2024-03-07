---
date: 2019-12-12T00:00:00-00:00
title: "Advanced Security Features (CLOUD)"
linkTitle: "Advanced Security Features"
weight: 8
description: >
  Methods to protect your Anka Build Cloud Controller & Registry.
---

## Overview

- These features are currently only available for our `Enterprise` tier customers.

- You must have all Nodes licensed with Enterprise or higher for these features to enable.

{{< hint warning >}}
We highly recommend that [HTTPS/TLS]({{< relref "anka-build-cloud/Advanced Security Features/tls.md" >}}) is enabled before using any of these features.
{{< /hint >}}

### Authentication

We provide several Authentication methods for the Anka Build Cloud. They are split into **Human** and **Programmatic Access**.

- Authentication features are enabled by adding `ANKA_ENABLE_AUTH` to your Controller and/or Registry configuration and setting it to `true`.

- Each one of these methods requires [Root Token Authentication]({{< relref "anka-build-cloud/Advanced Security Features/root-token-authentication.md" >}}) is enabled first.

#### Human Access

These methods are available to a human through a browser to the Controller UI/Dashboard.

- [Root Token]({{< relref "anka-build-cloud/Advanced Security Features/root-token-authentication.md" >}})
- [OIDC / SSO]({{< relref "anka-build-cloud/Advanced Security Features/openid-authentication.md" >}})

#### Programmatic Access

These methods are available for users and scripts that need to use the Controller API, Registry API, or are using the Anka CLI. They can also be used in our CI Integrations.

- [UAK+TAP]({{< relref "anka-build-cloud/Advanced Security Features/uak-tap-authentication.md" >}})
  - Anka CLI: through `ankacluster join --api-*` and `anka registry --api-*` options
  - Plugins / Integrations: No support yet for Jenkins, TeamCity, etc. Coming soon.
  - API: Full support.
- [MTLS / Certificates]({{< relref "anka-build-cloud/Advanced Security Features/certificate-authentication.md" >}})
  - Anka CLI: through `ankalcluster join --cert`, `--cert-key`, etc.
  - Plugins / Integrations: Full support across all tools.
  - API: Full support.

---

### Authorization

{{< include file="_partials/anka-build-cloud/advanced-security-features/authorization.md" >}}
