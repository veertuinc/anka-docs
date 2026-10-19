---
title: "Templates and tags"
linkTitle: "Templates and tags"
weight: 1
description: >
  Templates, full tags, OCI tags, base tags, and push modes
---

## Terms

| Term | Meaning |
|------|---------|
| **Template** | A VM that has at least one tag. In a registry, the template holds the tags for that VM. |
| **Tag** | A saved state of a template, such as `vanilla` or `v1`. |
| **Base tag** | The tag that a newer tag builds on. If you push `xcode-27.1` after `vanilla` on the same VM, `vanilla` is the base tag of `xcode-27.1`. |
| **Full tag** | A tag that includes its base tags and the macOS disk. The Anka Registry stores only full tags. An OCI registry stores a full tag when you push with `push_mode` `1`. |
| **OCI tag** | A set of file layers in an OCI registry. You make one when you push with `push_mode` `2` or `--thin`, or when you push an archive. Public OCI artifacts, such as Homebrew bottles, are also OCI tags. See [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}). |
| **Ankafile** | A file that lists the macOS version and the OCI tags to pull, in order. `anka create` builds one VM from it. See [Creating VMs with an Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}). |

Local tags, clones, and how clones share disk layers are in [VM clones, tags, and templates]({{< relref "anka-virtualization-cli/getting-started/creating-vms/vm-clones-and-tags.md" >}}).

## Anka Registry tags and OCI tags

{{< include file="_partials/templates-and-registries/oci-tags/_oci-tags-vs-anka-registry-tags.md" >}}

## Push modes

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-oci-tags.md" >}}

### `push_mode` `1`

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-mode-1.md" >}}

### `push_mode` `2`

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-mode-2.md" >}}

### Set the push mode

{{< include file="_partials/templates-and-registries/oci-tags/push/_set-push-mode.md" >}}

### Exclude files from a push

{{< include file="_partials/templates-and-registries/oci-tags/push/_exclude-files-with-ankaignore.md" >}}

## Thin pushes and pulls

`--thin` pushes or pulls only the last layer of a tag. The base tags stay out. `--thin` works with `push_mode` `1` and with `push_mode` `2`. Like `push_mode`, `--thin` applies only to an OCI registry.

For an example, see [Push OCI tags with `--thin`]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#with---thin" >}}).
