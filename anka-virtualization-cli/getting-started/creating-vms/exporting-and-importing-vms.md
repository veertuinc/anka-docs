---
title: "Exporting and importing VMs"
linkTitle: "Export and import"
weight: 5
description: >
  Move VM archives between hosts with anka export and anka import
aliases:
  - "/anka-virtualization-cli/getting-started/creating-vms/#exporting-and-importing-vms"
  - "/anka-virtualization-cli/getting-started/exporting-and-importing-vms/"
---

Use `anka export` and `anka import` to archive a VM and move it to another host or user without a registry.

## Export

{{< include file="_partials/anka-virtualization-cli/command-line-reference/export/_extra.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/export/_index.md" >}}

## Import

{{< include file="_partials/anka-virtualization-cli/command-line-reference/import/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/import/_extra.md" >}}

## Related

- [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}) for team-wide template storage
- [OCI Registries]({{< relref "anka-virtualization-cli/working-with-oci-registries.md" >}}) (CLI only; not Build Cloud)
