---
title: "Preparing a build VM"
linkTitle: "Preparing a build VM"
weight: 5
description: >
  Build a VM from OCI tags on one producer host, or on each runner with an Ankafile
---

{{< include file="_partials/templates-and-registries/preparing-a-build-vm/_preparing-a-build-vm.md" >}}

## On a producer host

{{< include file="_partials/templates-and-registries/preparing-a-build-vm/_on-a-producer-host.md" >}}

## On each runner with an Ankafile

An Ankafile stores the same steps, including the OCI tag order. Each runner creates the VM from that file, so you do not need a producer host.

```text
FROM darwin:27.0.1
FROM http://registry.example.com/v2/xcode:27.1
FROM https://ghcr.io/homebrew/core/cmake:4.4.4
```

```bash
❯ anka create build_vm ./Ankafile
```

For all Ankafile instructions, see [Creating VMs with an Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}).
