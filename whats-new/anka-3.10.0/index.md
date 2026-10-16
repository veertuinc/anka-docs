---
date: 2026-10-09T01:00:00-00:00
title: "Anka Virtualization 3.10.0"
---

## USB Passthrough for Silicon

{{< include file="_partials/anka-virtualization-cli/working-with-usb-devices/_usb-passthrough-for-silicon.md" >}}

## `anka create` reuses a macOS build without needing an IPSW

{{< include file="_partials/anka-virtualization-cli/getting-started/creating-vms/_create-reuses-macos-build.md" >}}

## Supporting OCI Layers

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/_overview.md" >}}

### Pull OCI tags

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/_pull-oci-tags.md" >}}

### Push OCI tags

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-oci-tags.md" >}}

#### `push_mode` `1`

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-mode-1.md" >}}

#### `push_mode` `2`

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-mode-2.md" >}}

#### Example

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/example/_example.md" >}}

##### Without `--thin`

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/example/_without-thin.md" >}}

##### With `--thin`

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/example/_with-thin.md" >}}

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/example/_push-after-thin-pull.md" >}}

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-onto-existing-template.md" >}}

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/_pull-url-only-with-default-vmid.md" >}}

### Export and import an OCI tag

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/_export-and-import.md" >}}

### Add an archive to an existing OCI tag

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/append-archive/_add-archive-to-existing-tag.md" >}}

#### Remove a file with a whiteout

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/append-archive/_remove-file-with-whiteout.md" >}}

### Preparing a build VM

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/preparing-a-build-vm/_preparing-a-build-vm.md" >}}

#### On a producer host

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/preparing-a-build-vm/_on-a-producer-host.md" >}}

#### With an Ankafile

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/preparing-a-build-vm/_with-an-ankafile.md" >}}
