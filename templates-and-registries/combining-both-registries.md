---
title: "Combining both registries"
linkTitle: "Combining both registries"
weight: 4
description: >
  Keep the macOS base template in the Anka Registry and each tool as a thin tag in an OCI registry
---

You can use the Anka Registry and an OCI registry together:

- The Anka Registry stores the macOS base template as a full tag. The Build Cloud Controller can use it.
- The OCI registry stores each tool as a small thin tag. You can pull the same tool tag onto templates for other macOS versions.

## Rules

- Pull the full tag from the Anka Registry first. Then pull the thin tags. A pull of a full tag removes the thin tags that you pulled before it.
- `--thin` and `push_mode` apply only to the pushes to the OCI registry. Use `--thin` for tool tags. A push to the Anka Registry always stores the full tag.
- The Anka Registry does not store thin tags.

For all differences, see [Full tags and thin tags]({{< relref "templates-and-registries/templates-and-tags.md#full-tags-and-thin-tags" >}}).

## Example

### Push the macOS base tag to the Anka Registry

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_base-tag-to-anka-registry.md" >}}

### Push the tool tags to the OCI registry

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_tool-tags.md" >}}

For the two forms of the `cmake` push, see [Without `--thin`]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#without---thin" >}}) and [With `--thin`]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#with---thin" >}}).

## Publish a template for Anka Build Cloud

The Build Cloud Controller cannot pull OCI tags, full or thin. Build the template on a producer host instead:

1. Pull the macOS base tag (a full tag) from the Anka Registry.
2. Pull each thin tag onto that VM, in order.
3. Push the finished VM to the Anka Registry. The Anka Registry stores it as a full tag.

The Controller can then start VMs from that template, the same as from any other template. For the commands, see [Preparing a build VM]({{< relref "templates-and-registries/preparing-a-build-vm.md" >}}).
