---
date: 2026-10-09T01:00:00-00:00
title: "Anka Virtualization 3.10.0"
---

## USB Passthrough for Silicon

{{< include file="_partials/anka-virtualization-cli/working-with-usb-devices/_usb-passthrough-for-silicon.md" >}}

## `anka create` reuses a macOS build without needing an IPSW

{{< include file="_partials/anka-virtualization-cli/getting-started/creating-vms/_create-reuses-macos-build.md" >}}

## Supporting OCI Container Registry Protocol

{{< include file="_partials/templates-and-registries/oci-tags/_overview.md" >}}

{{< include file="_partials/templates-and-registries/oci-tags/_oci-tags-vs-anka-registry-tags.md" >}}

### Exclude files from thin tags with `ankaignore`

{{< include file="_partials/templates-and-registries/oci-tags/push/_exclude-files-with-ankaignore.md" >}}

### Learn more

- [Templates and Registries]({{< relref "templates-and-registries/_index.md" >}}): choose the Anka Registry, an OCI registry, or both.
- [Templates and tags]({{< relref "templates-and-registries/templates-and-tags.md" >}}): full tags, thin tags, `--thin`, and `push_mode`.
- [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}): pull and push full and thin OCI tags, with examples.
- [Appending archives to OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/appending-archives.md" >}}): add files to a tag, or remove them with a whiteout.
- [Moving to OCI tags]({{< relref "templates-and-registries/moving-to-oci-tags.md" >}}): move the tool layers of your Anka Registry templates to thin tags, one step at a time.
- [Combining both registries]({{< relref "templates-and-registries/combining-both-registries.md" >}}): keep the macOS base in the Anka Registry and tools as thin tags.
- [Preparing a build VM]({{< relref "templates-and-registries/preparing-a-build-vm.md" >}}): build a VM from a full base tag and thin tags on a producer host or on each runner.
- [Creating VMs with an Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}): create a VM from a file that lists the thin tags.
- [Export and import an OCI tag]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md#export-and-import-an-oci-tag" >}}): move an OCI tag without a registry.
- [Edit a layer before you push it]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#edit-a-layer-before-you-push-it" >}}): browse and change an OCI tag layer with [anka-oci-tag-editor](https://github.com/veertuinc/anka-oci-tag-editor).

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
