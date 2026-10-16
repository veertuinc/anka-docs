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

## Export and import an OCI tag

{{< include file="_partials/templates-and-registries/oci-tags/_export-and-import.md" >}}

## Related

- [Templates and Registries]({{< relref "templates-and-registries/_index.md" >}}) to store templates in the Anka Registry or in an OCI registry
- [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}})
