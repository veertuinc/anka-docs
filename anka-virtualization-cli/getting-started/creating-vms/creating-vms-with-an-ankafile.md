---
title: "Creating VMs with an Ankafile"
linkTitle: "Ankafile"
weight: 3
description: >
  Create a VM from an Ankafile that lists the macOS version and the thin tags to pull (Anka 3.10.0 and later)
---

## Before you start

An Ankafile builds a VM from [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}). The first `FROM` creates macOS or pulls a full tag. Each later `FROM <registry-url>` line pulls one thin tag onto the VM. Read these pages first:

- [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}): what full and thin OCI tags are, and how a pull stacks a thin tag on a VM.
- [Templates and tags]({{< relref "templates-and-registries/templates-and-tags.md" >}}): how full tags and thin tags are different, and why the pull order matters.
- [Using an OCI registry]({{< relref "templates-and-registries/using-an-oci-registry/_index.md" >}}): how to give Anka access to a private registry.
- [Reuse a macOS build]({{< relref "anka-virtualization-cli/getting-started/creating-vms/_index.md#reuse-a-macos-build" >}}): how `FROM darwin:` reuses the macOS install.
- [Preparing a build VM]({{< relref "templates-and-registries/preparing-a-build-vm.md" >}}): when to use an Ankafile on each runner, and when to use a producer host.

## Ankafile reference

{{< include file="_partials/anka-virtualization-cli/getting-started/creating-vms/_create-with-an-ankafile.md" >}}
