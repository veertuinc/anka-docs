---
title: "Modifying your VM Template"
linkTitle: "Modifying your VM Template"
weight: 4
description: >
  Modify your VM Template using the Anka Virtualization CLI
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "docs/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've got an active license]({{< relref "docs/Licensing/_index.md" >}})
3. [You've created your first VM Template]({{< relref "docs/Getting Started/creating-your-first-vm.md" >}})

---

**The rest of this Getting Started guide focuses heavily on the Anka Virtualization CLI (Command-Line Interface). These will be performed from within your [macOS Terminal](https://support.apple.com/guide/terminal/welcome/mac). For all available CLI commands, flags, and options, see the [Command Reference]({{< relref "docs/Anka Virtualization/command-reference.md" >}}).**

{{< include file="shared/content/en/docs/Anka Virtualization/partials/modify/_index.md" >}}

{{< include file="shared/content/en/docs/Anka Virtualization/partials/modify/set/_index.md" >}}

## Increase your VM's disk space with `hard-drive`

Change the disk space on an existing VM with the following commands:

```shell
anka modify {vmNameOrUUID} set hard-drive --size 100GB
anka run --no-volume {vmNameOrUUID} diskutil apfs resizeContainer disk1 0
```

> It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM Template with a smaller amount of available disk and then increase in subsequent Tags.

## Changing your VM's network configuration with `network-card`

Depending on your network topology, there are instances where you might need to use a bridge mode and assign your VM a unique IP address instead of the default shared IP of the host:

{{< include file="shared/content/en/docs/Anka Virtualization/partials/modify/set/network-card/_index.md" >}}

{{< include file="shared/content/en/docs/Anka Virtualization/partials/modify/set/network-card/_example.md" >}}


## Set up your VM for external access with `port-forwarding`

If you wish for the VM to be accessible to other machines on your network, and are using a Shared network mode (the default), you will need to setup port forwarding:

{{< include file="shared/content/en/docs/Anka Virtualization/partials/modify/add/port-forwarding/_example.md" >}}

The VM can then be accessed using: `ssh anka@{theHostRunningTheVMsIP} -p {host_port}`

> Unless you customize the host_port (with `anka config portfwd_base`), we default to using the 10000-10XXX port range. If you start a VM it will get 10000 unless a VM already exist. The existing VM using the 10000 port will cause an automatic assignment of 10001 to the newly started VM. _Ensure that both the Controller server and any CI servers (where you host Jenkins for example) can reach the Node's ports in your firewall rules._

## Modify the hardware UUID and Serial with `custom-variable`

At times you might need to modify the hardware serial or UUID to run proper builds/tests that require specific hardware to function properly:

{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/custom-variable/_index.md" >}}
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/custom-variable/_example.md" >}}