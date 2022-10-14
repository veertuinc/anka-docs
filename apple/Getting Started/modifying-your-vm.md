---
title: "Modifying your VM"
linkTitle: "Modifying your VM"
weight: 4
description: >
  Modify your VM using the Anka Virtualization CLI
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "apple/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created your first VM]({{< relref "apple/Getting Started/creating-your-first-vm.md" >}})

---

{{< hint warning >}}
The rest of this Getting Started guide focuses heavily on the (Command-Line Interface). These will be performed from within your [macOS Terminal](https://support.apple.com/guide/terminal/welcome/mac). For all available CLI commands, flags, and options, see the [Command Reference]({{< relref "apple/command-line-reference.md" >}}).
{{< /hint >}}

{{< include file="_partials/apple/Anka Virtualization/modify/_index.md" >}}

## Recommended VM Resources

### Mac mini (M1, 2020, 8-core/4-performance + 4-efficiency, 8 or 16GB RAM)

- 1VM (recommended) - CPUs: 6-cores & RAM: ( totalRAMGB - 2GB )
- 2VMs  (not recommended) - CPUs: 4-cores & RAM: ( (totalRAMGB / 2) - 2GB )

### Mac Studio (M1 Max, 2022, 10-core/8-performance + 2-efficiency, 32 or 64GB RAM)

- 1VM  - CPUs: 8-cores & RAM: ( totalRAMGB - 2GB )
- 2VMs  - CPUs: 6,8-cores & RAM: ( (totalRAMGB / 2) - 2GB )

### Mac Studio (M1 Ultra, 2022, 20-core/16-performance + 4-efficiency, 64 or 128GB RAM)

- 1VM  - CPUs: 8-cores & RAM: ( totalRAMGB - 2GB )
- 2VMs  - CPUs: 8-cores & RAM: ( (totalRAMGB / 2) - 2GB )

{{< hint info >}}
Due to Ultra using the NUMA architecture, VMs/virtualization will only ever use 8 performance cores at a time.
{{< /hint >}}

## Common Examples

### VM Resources

#### CPU

{{< include file="_partials/apple/Anka Virtualization/modify/cpu/_index.md" >}}
{{< include file="_partials/apple/Anka Virtualization/modify/cpu/_example.md" >}}

#### RAM

{{< include file="_partials/apple/Anka Virtualization/modify/ram/_index.md" >}}
{{< include file="_partials/apple/Anka Virtualization/modify/ram/_example.md" >}}

#### DISK

{{< include file="_partials/apple/Anka Virtualization/modify/disk/_index.md" >}}
{{< include file="_partials/apple/Anka Virtualization/modify/disk/_extra.md" >}}
{{< include file="_partials/apple/Anka Virtualization/modify/disk/_example.md" >}}


### Port forwarding from guest to host

{{< include file="_partials/apple/Anka Virtualization/modify/port/_index.md" >}}

{{< include file="_partials/apple/Anka Virtualization/modify/port/_example.md" >}}

### Changing your VM's network configuration

Depending on your network topology, there are instances where you might need to use a bridge mode and assign your VM a unique IP address instead of the default shared IP of the host:

{{< include file="_partials/apple/Anka Virtualization/modify/network/_index.md" >}}

{{< include file="_partials/apple/Anka Virtualization/modify/network/_extra.md" >}}

{{< include file="_partials/apple/Anka Virtualization/modify/network/_example.md" >}}

---

## What's next?

- [Understanding VM Networking]({{< relref "apple/Getting Started/understanding-vm-networking.md" >}})
