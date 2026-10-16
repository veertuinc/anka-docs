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

### Pulling OCI tags

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/_pull-oci-tags.md" >}}

### Pushing OCI tags

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-oci-tags.md" >}}

#### `push_mode` `1`

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-mode-1.md" >}}

#### `push_mode` `2`

{{< include file="_partials/anka-virtualization-cli/working-with-oci-registries/oci-tags/push/_push-mode-2.md" >}}

#### Examples

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

## System proxy and PAC support

CLI HTTP requests now use the macOS system proxy settings for the request URL. VM network traffic does not use this proxy.

These requests use the system proxy:

- Registry HTTP, including `anka pull`, `anka push`, the OCI registry API, and the registry handshake.
- An HTTP or HTTPS installer download during `anka create`.
- The tools feed download during `anka create`.
- The license key lookup from `anka license show -k`.

Anka supports these proxy types:

- HTTP
- HTTPS
- SOCKS
- A Proxy Auto-Configuration (PAC) URL. Anka downloads the PAC script and uses the proxy that the script returns.

Hosts in the system proxy exception list do not use the proxy. A list entry can be `*`, a host name, a domain suffix, or an IPv4 or IPv6 address with a CIDR mask.
