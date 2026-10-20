---
title: "OCI tags"
linkTitle: "OCI tags"
weight: 1
description: >
  Pull file layers from any OCI registry onto a VM, and push your own tools as OCI tags (Anka 3.10.0 and later)
---

{{< include file="_partials/templates-and-registries/oci-tags/_overview.md" >}}

{{< rawhtml >}}<center>{{< /rawhtml >}}
![The same OCI tags stacked on VMs with different macOS versions]({{< siteurl >}}images/templates-and-registries/oci-tags-on-a-vm.webp)
{{< rawhtml >}}</center>{{< /rawhtml >}}

Before you mix OCI tags with Anka Registry tags, read [Anka Registry tags and OCI tags]({{< relref "templates-and-registries/templates-and-tags.md#anka-registry-tags-and-oci-tags" >}}).

## Pull OCI tags

{{< include file="_partials/templates-and-registries/oci-tags/_pull-oci-tags.md" >}}

### Pull without a VM name

{{< include file="_partials/templates-and-registries/oci-tags/_pull-url-only-with-default-vmid.md" >}}

## Push OCI tags

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-oci-tags.md" >}}

The example below leaves `push_mode` at `1` and uses `--thin` on the tool tags. For `push_mode`, see [Push modes]({{< relref "templates-and-registries/templates-and-tags.md#push-modes" >}}).

### Push the macOS base tag

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_base-tag-to-oci-registry.md" >}}

{{< hint info >}}
To keep the macOS base tag in the Anka Registry instead, see [Combining both registries]({{< relref "templates-and-registries/combining-both-registries.md" >}}).
{{< /hint >}}

### Push the tool tags

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_tool-tags.md" >}}

#### With `--thin`

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_with-thin.md" >}}

#### Without `--thin`

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_without-thin.md" >}}

{{< include file="_partials/templates-and-registries/oci-tags/push/example/_push-after-thin-pull.md" >}}

## Push onto an existing template

{{< include file="_partials/templates-and-registries/oci-tags/push/_push-onto-existing-template.md" >}}

## Move OCI tags without a registry

To move an OCI tag between hosts with `anka export` and `anka import`, see [Export and import an OCI tag]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md#export-and-import-an-oci-tag" >}}).

## Edit a layer before you push it

[anka-oci-tag-editor](https://github.com/veertuinc/anka-oci-tag-editor) is a separate terminal tool. It lets you browse an OCI tag layer and change it before you push it. You can remove files, add files from the host, and add [whiteouts]({{< relref "templates-and-registries/using-an-oci-registry/appending-archives.md#remove-a-file-with-a-whiteout" >}}) that delete a path from the base. You build the tool from that repository.

You can use it in two ways:

- **On an exported archive.** Export the layer with `anka export --layer`, open the `.tar.gz` in the editor, then import the edited archive.
- **On a stopped VM.** Run `anka-oci-tag-editor --vm <name>`. The editor opens the top layer of the VM's current OCI tag. When you save, it commits the edits as a new local OCI tag.

## Build a VM from OCI tags

- [Creating VMs with an Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}): list the macOS version and the OCI tags in one file, and create the VM with one `anka create` command.
- [Preparing a build VM]({{< relref "templates-and-registries/preparing-a-build-vm.md" >}}): build the VM once on a producer host, or on each runner with an Ankafile.
