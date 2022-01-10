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

### With the UI

{{< hint warning >}}
The first beta release does not support mounting addons by starting from the UI. Please use the CLI for now.
{{< /hint >}}

### With the CLI

1. You’ll need to start the VM with `anka start -uv` to launch the viewer.

{{< hint warning >}}
`anka view` does not currently work post-start unless you started it with -v.
{{< /hint >}}

{{< include file="_partials/arm/Anka Virtualization/start/_index.md" >}}

2. Once inside, finish the macOS installation **and be sure to install the addons package through the disk we mount with `-u`**.

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

## VM Derivatives and Disk Optimization

You can easily create VM derivatives from a source VM using `anka clone`:

{{< include file="_partials/arm/Anka Virtualization/clone/_index.md" >}}

```bash
❯ anka list
+--------+--------------------------------------+----------------------+---------+
| name   | uuid                                 | creation_date        | status  |
+--------+--------------------------------------+----------------------+---------+
| 12.0.1 | e65a072f-0d4f-450b-964d-2be8d0d32c13 | Nov 19 08:02:33 2021 | stopped |
+--------+--------------------------------------+----------------------+---------+


❯ anka clone 12.0.1 12.0.1-xcode13
6070ee59-6c16-4c93-ba7a-122b66b1472a

❯ anka list
+----------------+--------------------------------------+----------------------+---------+
| name           | uuid                                 | creation_date        | status  |
+----------------+--------------------------------------+----------------------+---------+
| 12.0.1         | e65a072f-0d4f-450b-964d-2be8d0d32c13 | Nov 19 08:02:33 2021 | stopped |
+----------------+--------------------------------------+----------------------+---------+
| 12.0.1-xcode13 | 6070ee59-6c16-4c93-ba7a-122b66b1472a | Nov 19 08:02:33 2021 | stopped |
+----------------+--------------------------------------+----------------------+---------+
```

Anka VMs have an image structure which allows for sharing and disk optimization. When you create VM derivatives from a source VM, **this sharing only happens _after_ a specific "commit" action** using `anka push --local`:

{{< hint warning >}}
Important note about the Anka 2.X/Intel version: The sharing of images did not require a local commit/tag like Anka 3.X does.
{{< /hint >}}

{{< include file="_partials/arm/Anka Virtualization/push/_index.md" >}}

```bash
❯ anka list
+--------+--------------------------------------+----------------------+---------+
| name   | uuid                                 | creation_date        | status  |
+--------+--------------------------------------+----------------------+---------+
| 12.0.1 | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+--------+--------------------------------------+----------------------+---------+

❯ anka push --local --tag vanilla 12.0.1

❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
```

The above example shows the tag "vanilla" does not exist locally until we execute the `anka push --local`. Once tagged, we can then clone from it, or even create other tags on top.

Each cloned VM will share the underlying data of the original source VM we clone from. For example, if `VM1` has `1.ank` **image** on disk, and you clone from it to create `VM2`, the `VM2` will re-use `1.ank` and not double the disk usage with its own files.

{{< hint info >}}
Cloned VMs will not use any significant disk space until you start them. Once started, an empty image is added on top of existing and any changes macOS or you make are added to it.
{{< /hint >}}

{{< hint warning >}}
Existing blocks/data in previous images **are not** removed and will continue to take up the disk space. This is why we recommend you create a fresh/vanilla macOS VM to clone and create VM derivatives from.
{{< /hint >}}

{{< hint info >}}
To switch between tags locally, you can use the `anka pull --local --tag {targetTagname} {VMName}` command:

{{< include file="_partials/arm/Anka Virtualization/pull/_index.md" >}}

{{< /hint >}}

---

## What's next?

- [Acessing your VM]({{< relref "arm/Getting Started/accessing-your-vm.md" >}})
