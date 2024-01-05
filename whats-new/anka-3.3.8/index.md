---
date: 2023-12-04T01:00:00-00:00
title: "Anka Virtualization 3.3.8"
---

We are very excited to announce Anka Virtualization 3.3.8. In this version, you're going to find:

1. [Import and Export for VMs](#import-and-export-for-vms)

---

## Import and Export for VMs

You can now export and import your VMs to an archive to assist with moving VMs between hosts.

- Only available for Enterprise or Enterprise Plus licenses.
- A VM Template tag will not be included in the export/import. However the active tag for a VM is the state we will export. Future versions will support this and allow registry users to utilize export/import to migrate VM templates without `anka pull` and `anka push`.
- Underlying layer/data sharing between the VMs does not happen. Exporting a VM is performing a unique copy like `anka clone -c` would do.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/export/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/import/_index.md" >}}

