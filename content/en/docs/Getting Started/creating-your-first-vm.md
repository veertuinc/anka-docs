---
title: "Creating your first VM"
linkTitle: "Creating your first VM"
weight: 2
description: >
  Step by step on how to create your first VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "docs/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've got an active license]({{< relref "docs/Licensing/_index.md" >}})

## 1. Obtain the macOS installer

{{< include file="shared/content/en/docs/Getting Started/partials/_supported-macos-versions.md" >}}

{{< include file="shared/content/en/docs/Getting Started/partials/_obtain-macos-installer.md" >}}

> As an alternative to the macOS installer, [you can also use an iso]({{< relref "docs/Anka Virtualization/creating-a-vm-template-with-an-iso.md" >}})

## 2. Create your VM

> The Anka VM is pre-configured with a default administrative username `anka` and password `admin`. You can override this with ENVs: `ANKA_DEFAULT_USER` and `ANKA_DEFAULT_PASSWD`

> **Anka Develop license (default):** While you can create as many VMs as you wish, the free Anka Develop license only allows you to run one VM at a time and will only function on laptops (Macbook, Macbook Pro, and Macbook Air).

> **Anka Build license:** When determining how many cores or memory your VM needs, you can divide the number of VMs you plan on running simultaneously within a single machine by the total **virtual cores** it has. So, if I have 12vCPUs on my 6core Mac Mini, and I want to allow 2 VMs at once, I will set the VM Template/Tag to have 6cores (12 / 2). However, with RAM, you'll need to allow ~2GB of memory for the Anka Software and host ((totalRAM / 2)-1).

### Using the Anka UI

1. Click on **Create new VM**
2. Choose the installer (will automatically search /Applications), or drop it into the window
3. Click on Options and set any non-default values you want
![installer with pkg](/images/getting-started/creating-your-first-vm/create-vm-window-with-options.png)
4. **Please be patient while it's creating. This process can take up to half an hour.**

Once the VM template is created, you will see it on the sidebar.

![ui with vm in the sidebar list](/images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

### Using the Anka CLI

{{< include file="shared/content/en/docs/Anka Virtualization/partials/create/_index.md" >}}

{{< include file="shared/content/en/docs/Anka Virtualization/partials/create/_example.md" >}}

## Listing available VMs in the CLI

```shell
❯ anka list
+------------------------------------------------+--------------------------------------+--------------+------------------------+
| name                                           | uuid                                 | created      | status                 |
+================================================+======================================+==============+========================+
| 10.15.5 (base:port-forward-22:brew-git:gitlab) | c0847bc9-5d2d-4dbc-ba6a-240f7ff08032 | Jul 13 16:40 | suspended Sep 01 20:18 |
+------------------------------------------------+--------------------------------------+--------------+------------------------+
| 10.15.6                                        | 5bba6eea-202e-45ed-9567-d7f55090d049 | Sep 02 10:17 | stopped Sep 02 13:37   |
+------------------------------------------------+--------------------------------------+--------------+------------------------+
| 11.0.1                                         | 2bb1b152-9996-41d6-804a-a429973c13bc | Sep 01 18:45 | stopped Sep 02 14:28   |
+------------------------------------------------+--------------------------------------+--------------+------------------------+
| test                                           | 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 | Sep 03 11:30 | running                |
+------------------------------------------------+--------------------------------------+--------------+------------------------+
```

## Deleting a VM

### Using the Anka UI

![edit menu delete](/images/getting-started/creating-your-first-vm/edit-menu-delete.png)

### Using the Anka CLI

```shell
❯ anka delete test
are you sure you want to delete vm 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 test [y/N]:
```

---

## (Anka Build license + Cloud) Understanding VM Templates, Tags, and Disk Usage

{{< include file="./shared/content/en/docs/Getting Started/partials/_understanding-templates-and-tags.md" >}}
