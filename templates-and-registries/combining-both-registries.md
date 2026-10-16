---
title: "Combining both registries"
linkTitle: "Combining both registries"
weight: 4
description: >
  Keep the macOS base template in the Anka Registry and each tool as an OCI tag in an OCI registry
---

You can use the Anka Registry and an OCI registry together:

- The Anka Registry stores the macOS base template as a full tag. The Build Cloud Controller can use it.
- The OCI registry stores each tool as a small OCI tag. You can pull the same tool tag onto templates for other macOS versions.

## Rules

- Pull the Anka Registry tag first. Then pull the OCI tags. A pull of an Anka Registry tag removes the OCI tags that you pulled before it.
- `push_mode` and `--thin` apply only to the pushes to the OCI registry. A push to the Anka Registry always stores the full tag.
- The Anka Registry does not store OCI tags.

For all differences, see [Anka Registry tags and OCI tags]({{< relref "templates-and-registries/templates-and-tags.md#anka-registry-tags-and-oci-tags" >}}).

## Example

### Push the macOS base tag to the Anka Registry

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_base-tag-to-anka-registry.md" >}}

### Push the tool tags to the OCI registry

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_tool-tags.md" >}}

For the two forms of the `cmake` push, see [Without `--thin`]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#without---thin" >}}) and [With `--thin`]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#with---thin" >}}).

## Publish a template for Anka Build Cloud

The Build Cloud Controller cannot pull OCI tags. Build the template on a producer host instead:

1. Pull the macOS base tag from the Anka Registry.
2. Pull each OCI tag onto that VM, in order.
3. Push the finished VM to the Anka Registry.

The Controller can then start VMs from that template, the same as from any other template. For the commands, see [Preparing a build VM]({{< relref "templates-and-registries/preparing-a-build-vm.md" >}}).
