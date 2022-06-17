---
date: 2019-12-12T00:00:00-00:00
title: "HTTPS/TLS"
weight: 1
description: >
  How to protect your Controller UI, API, and and Registry API with HTTPS/TLS.

---

## Requirements

1. A Root CA certificate. For more information about CAs, see https://en.wikipedia.org/wiki/Certificate_authority. Usually provided by your organization or where you obtain your certificate signing. We will generate a self-signed one in this guide and refer to this as **anka-ca-crt.pem** and **anka-ca-key.pem**.

{{< include file="_partials/Anka Build Cloud/advanced-security-features/tls.md" >}}
