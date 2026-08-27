---
title: "VM clones, tags, and templates"
linkTitle: "Clones and tags"
weight: 4
description: >
  Local tags, cloning, and how VM templates relate to Anka Build Cloud
aliases:
  - "/anka-virtualization-cli/getting-started/creating-vms/#vm-clones"
  - "/anka-virtualization-cli/getting-started/vm-clones-and-tags/"
---

## Terms

| Term | Meaning |
|------|---------|
| **VM** | A macOS virtual machine on the host (`anka list`) |
| **Tag** | A named snapshot/version of a VM (`vanilla`, `v2`, and similar). Shown as `name (tag)` in `anka list` |
| **Template** | A VM (or registry object) used as the basis for clones and CI images. In Build Cloud, templates live in the [Registry]({{< relref "anka-build-cloud/getting-started/registry-vm-templates-and-tags.md" >}}) |

Clones share disk layers with the source **only if** the source has a tag. Create a local tag with `anka push --local --tag {name}` before cloning, or push to the Anka Registry with `anka registry push`.

## Disk optimization (Anka 3)

In Anka 2, clones shared underlying image files automatically. In Anka 3, sharing requires a **tag on the source** before you clone. Use `anka push --local` or `anka registry push` to create that tag. Clones never modify the source VM state.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/push/_index.md" >}}

```bash
❯ anka list
+--------+--------------------------------------+----------------------+---------+
| name   | uuid                                 | creation_date        | status  |
+--------+--------------------------------------+----------------------+---------+
| 12.0.1 | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+--------+--------------------------------------+----------------------+---------+

❯ anka push --local --tag vanilla 12.0.1

❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
```

{{< hint info >}}
Clones use little disk until started. On start, Anka adds a writable layer on top of shared layers; guest changes go there.
{{< /hint >}}

Switch between local tags:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/pull/_index.md" >}}

## Cloning

Create a new VM from a source and its current state:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/clone/_index.md" >}}

1. **Shallow clone:** `anka clone {source} {dest}` — new name and UUID; shares layers if the source has a tag.
2. **Full clone:** `anka clone --copy {source} {dest}` — merges layers into a standalone copy (often uses more disk; cannot share layers with other VMs).

```bash
❯ anka clone 12.0.1 12.0.1-xcode13
6070ee59-6c16-4c93-ba7a-122b66b1472a
```

## VM templates

{{< include file="_partials/anka-build-cloud/vm-templates.md" >}}

Push templates to the registry: [Registry VM Templates and Tags]({{< relref "anka-build-cloud/getting-started/registry-vm-templates-and-tags.md" >}}).
