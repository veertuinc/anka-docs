---
title: "Templates and tags"
linkTitle: "Templates and tags"
weight: 1
description: >
  Templates, full tags, thin tags, OCI tags, base tags, and push modes
---

## Terms

| Term | Meaning |
|------|---------|
| **Template** | A VM that has at least one tag. In a registry, the template holds the tags for that VM. |
| **Tag** | A saved state of a template, such as `vanilla` or `v1`. A tag is a full tag or a thin tag. |
| **Base tag** | The tag that a newer tag builds on. If you push `xcode-27.1` after `vanilla` on the same VM, `vanilla` is the base tag of `xcode-27.1`. |
| **Full tag** | A tag that includes its base tags and the macOS disk. A VM can start from a full tag alone. The Anka Registry stores only full tags. An OCI registry stores a full tag when you push with `push_mode` `1` and without `--thin`. |
| **Thin tag** | A tag that holds only file layers. It does not include the macOS disk, so you pull it onto a VM that already exists. You make one when you push with `--thin`, or with `push_mode` `2`, or when you push an archive. Public OCI artifacts, such as Homebrew bottles and Hugging Face GGUF models, are also thin tags. Only an OCI registry stores thin tags. |
| **OCI tag** | A tag in an OCI registry. An OCI tag is a full tag or a thin tag. See [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}). |
| **Ankafile** | A file that lists the macOS version and the thin tags to pull, in order. `anka create` builds one VM from it. See [Creating VMs with an Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}). |

Local tags, clones, and how clones share disk layers are in [VM clones, tags, and templates]({{< relref "anka-virtualization-cli/getting-started/creating-vms/vm-clones-and-tags.md" >}}).

## Full tags and thin tags

{{< include file="_partials/templates-and-registries/oci-tags/_oci-tags-vs-anka-registry-tags.md" >}}

## Thin pushes and pulls

Use `--thin` when you push or pull a tool as a thin tag. `--thin` uses only the last layer of a tag. The base tags stay out. Leave `push_mode` at `1` (the default). `--thin` applies only to an OCI registry.

For an example, see [Push OCI tags with `--thin`]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#with---thin" >}}).

## Push modes

Prefer `--thin` for tool tags. Use `push_mode` only when you need a different store of the file changes. `push_mode` applies only to an OCI registry.

### `push_mode` `1`

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-mode-1.md" >}}

### `push_mode` `2`

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-mode-2.md" >}}

### Set the push mode

{{< include file="_partials/templates-and-registries/oci-tags/push/_set-push-mode.md" >}}

### Exclude files from a push

{{< include file="_partials/templates-and-registries/oci-tags/push/_exclude-files-with-ankaignore.md" >}}
