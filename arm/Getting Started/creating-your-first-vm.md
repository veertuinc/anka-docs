---
title: "Creating your first VM"
linkTitle: "Creating your first VM"
weight: 2
description: >
  Step by step on how to create your first VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "arm/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. The machine you wish to use is M1 and not Intel
3. The machine you wish to use has Monterey installed

## 1. Obtain the macOS installer

In the beta release of Anka 3.0, you will not be able to use installer .app files like you're used to with the Intel (2.X) versions of Anka. Anka 3.0 will download the latest Monterey version for you automatically.

## 2. Create your first VM Template

{{< hint warning >}}
Avoid the use of `sudo` commands as there are issues with non-sudo users launching VMs with sudo in the initial beta.
{{< /hint >}}

{{< hint info >}}
Creating a VM in Anka 3.0 differs from the Intel version. Anka 3.0 requires that you manually set up macOS inside of the VM. See step #3 below.
{{< /hint >}}

{{< include file="_partials/arm/Getting Started/_supported-macos-versions.md" >}}

### Using the Anka UI

1. Click on **Create new VM**.
2. Click on Options and set any non-default values you want.
![installer with pkg]({{< siteurl >}}images/arm/getting-started/creating-your-first-vm/ui.png)
3. Be patient while it's creating.

Once the VM template is created, you will see it on the sidebar.

{{< hint warning >}}
Suspending VMs is currently not available.
{{< /hint >}}

![ui with vm in the sidebar list]({{< siteurl >}}images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

---

### Using the Anka CLI

{{< include file="_partials/arm/Anka Virtualization/create/_index.md" >}}

{{< include file="_partials/arm/Anka Virtualization/create/_example.md" >}}

## 3. Start the VM and finish the macOS install

{{< hint info >}}
Be sure to install the addons package. This will be mounted as a disk into the VM (`-u`). Once they're installed, your VM is ready to use!
{{< /hint >}}

### With the UI

{{< hint warning >}}
The first beta release does not support mounting addons by starting from the UI. Please use the CLI for now.
{{< /hint >}}

### With the CLI

You’ll need to start the VM with `anka start -uv` to launch the viewer;
`anka view` does not currently work post-start unless you started it with -v.

{{< include file="_partials/arm/Anka Virtualization/start/_index.md" >}}

---
## Listing available VMs in the CLI

{{< include file="_partials/arm/Anka Virtualization/list/_example.md" >}}

## Deleting a VM

### Using the Anka UI

![edit menu delete]({{< siteurl >}}images/getting-started/creating-your-first-vm/edit-menu-delete.png)

### Using the Anka CLI

```shell
❯ anka delete test
are you sure you want to delete vm 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 test [y/N]:
```

---

## What's next?

- [Acessing your VM]({{< relref "arm/Getting Started/accessing-your-vm.md" >}})
