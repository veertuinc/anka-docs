---
title: "Modifying your VM"
linkTitle: "Modifying your VM"
weight: 4
description: >
  Modify your VM using the Anka Virtualization CLI
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "arm/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created your first VM]({{< relref "arm/Getting Started/creating-your-first-vm.md" >}})

---

{{< hint warning >}}
The rest of this Getting Started guide focuses heavily on the Anka Virtualization CLI (Command-Line Interface). These will be performed from within your [macOS Terminal](https://support.apple.com/guide/terminal/welcome/mac). For all available CLI commands, flags, and options, see the [Command Reference]({{< relref "arm/Anka Virtualization/command-reference.md" >}}).
{{< /hint >}}

{{< include file="_partials/arm/Anka Virtualization/modify/_index.md" >}}

{{< include file="_partials/arm/Anka Virtualization/modify/set/_index.md" >}}

## Common Examples

### Changing your VM's network configuration

Depending on your network topology, there are instances where you might need to use a bridge mode and assign your VM a unique IP address instead of the default shared IP of the host:

{{< include file="_partials/arm/Anka Virtualization/modify/network/_index.md" >}}

{{< include file="_partials/arm/Anka Virtualization/modify/network/_extra.md" >}}

{{< include file="_partials/arm/Anka Virtualization/modify/network/_example.md" >}}

---

## What's next?

- [Understanding VM Networking]({{< relref "arm/Getting Started/understanding-vm-networking.md" >}})
