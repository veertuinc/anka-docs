---
date: 2019-12-12T00:00:00-00:00
title: "Advanced Security Features (CLI)"
linkTitle: "Advanced Security Features"
weight: 8
description: >
  Advanced Security Feature for the Anka CLI (enterprise or higher license required).
---

{{< hint warning >}}
These features require a Enterprise or Enterprise Plus license.
{{< /hint >}}

## VM Networking

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/network/_index.md" >}}

### Block VM to VM and VM to Host communication

You may wish to disable the ability for VMs or VMs and the Host to communicate. This can be done with `--no-local` under `modify {VM} network`.

### IP Filtering Rules

{{< include file="_partials/anka-virtualization-cli/advanced-security-features/ip-filtering.md" >}}
