---
title: "Modifying your VM"
linkTitle: "Modifying your VM"
weight: 4
description: >
  Modify your VM using the Anka Virtualization CLI
---

**The rest of this Getting Started guide focuses heavily on the Anka Virtualization CLI (Command-Line Interface). These will be performed from within your [macOS Terminal](https://support.apple.com/guide/terminal/welcome/mac). For all available CLI commands, flags, and options, see the [Command Reference]({{< relref "intel/command-line-reference.md" >}}).**

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've got an active license]({{< relref "Licensing/_index.md" >}})
3. [You've created your first VM]({{< relref "intel/Getting Started/creating-your-first-vm.md" >}})

---

## Using the `modify` command

{{< include file="_partials/intel/Anka Virtualization/modify/_index.md" >}}

{{< include file="_partials/intel/Anka Virtualization/modify/set/_index.md" >}}

### Increase your VM's disk space with `hard-drive`

Change the disk space on an existing VM with the following commands:

```shell
anka modify {vmNameOrUUID} set hard-drive --size 100GB
anka run --no-volume {vmNameOrUUID} diskutil apfs resizeContainer disk1 0
```

> It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM with a smaller amount of available disk and then increase in subsequent Tags.

### Changing your VM's network configuration with `network-card`

Depending on your network topology, there are instances where you might need to use a bridge mode and assign your VM a unique IP address instead of the default shared IP of the host:

{{< include file="_partials/intel/Anka Virtualization/modify/set/network-card/_index.md" >}}

{{< include file="_partials/intel/Anka Virtualization/modify/set/network-card/_example.md" >}}


### Set up your VM for external access with `port-forwarding`

If you wish for the VM to be accessible to other machines on your network, and are using a Shared network mode (the default), you will need to setup port forwarding:

{{< include file="_partials/intel/Anka Virtualization/modify/add/port-forwarding/_example.md" >}}

The VM can then be accessed using: `ssh anka@{theHostRunningTheVMsIP} -p {host_port}`

> Unless you customize the host_port (with `anka config portfwd_base`), we default to using the 10000-10XXX port range. If you start a VM it will get 10000 unless a VM already exist. The existing VM using the 10000 port will cause an automatic assignment of 10001 to the newly started VM. _Ensure that both the Controller server and any CI servers (where you host Jenkins for example) can reach the Node's ports in your firewall rules._

### Modify the hardware UUID and Serial with `custom-variable`

At times you might need to modify the hardware serial or UUID to run proper builds/tests that require specific hardware to function properly:

{{< include file="_partials/intel/Anka Virtualization/modify/set/custom-variable/_index.md" >}}
{{< include file="_partials/intel/Anka Virtualization/modify/set/custom-variable/_example.md" >}}

## Running commands inside of the VM

{{< hint info >}}
We have a useful example bash script which shows how to automate the execution of commands to create the various VM Templates/Tags necessary for your project. You can find the script [here](https://github.com/veertuinc/getting-started/blob/master/create-vm-template-tags.bash). (requires a [registry]({{< relref "Anka Build Cloud/_index.md" >}}))
{{< /hint >}}

Installing dependencies, making OS or application level configuration changes, and even kicking off your build or test requires execution of commands inside of the VM. In the [previous section on accessing your VM]({{< relref "intel/Getting Started/starting-and-accessing-your-vm.md" >}}), we explained how you can [use Anka Run]({{< relref "intel/Getting Started/starting-and-accessing-your-vm.md" >}}) to execute commands from the host's terminal. Below are some examples, giving you an idea how to prepare the VM Template/Tag for your projects.

### Disable Spotlight/MDS to avoid it consuming CPU/RAM resources

{{< include file="_partials/intel/Getting Started/_disable_spotlight.md" >}}

It's always a good idea to run `anka run 12.1.0 bash -c "ps aux | head"` and ensure that there are no heavy CPU or RAM processes running.

## Stop or Suspend the VM

{{< hint info >}}
This is not necessary for `anka modify` commands.
{{< /hint >}}

Once you've finalized your changes inside of the VM, be sure to use `anka stop` or `anka suspend`.

{{< include file="_partials/intel/Anka Virtualization/stop/_index.md" >}}

{{< include file="_partials/intel/Anka Virtualization/suspend/_index.md" >}}

---

## What's next?

- [Understanding VM Networking]({{< relref "intel/Getting Started/understanding-vm-networking.md" >}})
