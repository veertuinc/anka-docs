---
date: 2023-03-07T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.33.0"
---

### Code/Explicit Flow OIDC support

{{< hint warning >}}
Customers must ensure their provider supports Code/Explicit Flow before upgrading.
{{< /hint >}}

{{< hint warning >}}
ENV `ANKA_OIDC_CACHE_TTL` has been deprecated.
{{< /hint >}}

Starting in this version we have removed the Implicit Flow support for OIDC users. This is a major change, but should modernize and increase the security drastically for your cloud. [Read more here.]({{< relref "anka-build-cloud/Advanced Security Features/openid-authentication.md" >}})

### New Permissions Management Panel

The previous `admin/ui` page has been replaced with a new `/#/permission-groups` location on the Controller, visible from the dashboard when logged in to a user with appropriate permissions to manage. There are also helper button on the right which will highlight the recommended permissions to add for the specific use-case you have (Node communication, API clients, etc).

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/new-permissions-management.png" >}}

### Support Certificate Auth with for NGINX Ingress in Kubernetes

{{< include file="_partials/anka-build-cloud/advanced-security-features/nginx-header-passthrough.md" >}}
