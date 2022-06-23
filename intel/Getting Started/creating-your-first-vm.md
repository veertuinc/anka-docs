---
title: "Creating your first VM"
linkTitle: "Creating your first VM"
weight: 2
description: >
  Step by step on how to create your first VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've got an active license]({{< relref "Licensing/_index.md" >}})
3. The machine you wish to use is using an Intel and not an Apple processor.

## 1. Obtain the macOS installer

{{< include file="_partials/intel/Getting Started/_supported-macos-versions.md" >}}

{{< include file="_partials/intel/Getting Started/_obtain-macos-installer.md" >}}

## 2. Create your VM

### Before you begin

{{< include file="_partials/intel/Anka Virtualization/create/_extra.md" >}}

### Using the Anka UI

1. Click on **Create new VM**
2. Choose the installer (will automatically search /Applications), or drop it into the window
3. Click on Options and set any non-default values you want
![installer with pkg]({{< siteurl >}}images/getting-started/creating-your-first-vm/create-vm-window-with-options.png)
4. **Please be patient while it's creating. This process can take up to half an hour.**

Once the VM template is created, you will see it on the sidebar.

![ui with vm in the sidebar list]({{< siteurl >}}images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

### Using the Anka CLI

{{< include file="_partials/intel/Anka Virtualization/create/_index.md" >}}

{{< include file="_partials/intel/Anka Virtualization/create/_example.md" >}}

## Listing available VMs in the CLI

{{< include file="_partials/intel/Anka Virtualization/list/_example.md" >}}

## Deleting a VM

### Using the Anka UI

![edit menu delete]({{< siteurl >}}images/getting-started/creating-your-first-vm/edit-menu-delete.png)

### Using the Anka CLI

```shell
❯ anka delete test
are you sure you want to delete vm 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 test [y/N]:
```

---

## (Anka Build license + Cloud) Understanding VM Templates, Tags, and Disk Usage

{{< include file="_partials/intel/Getting Started/_understanding-templates-and-tags.md" >}}

## Optimizing your VM

It's recommended that you disable Spotlight and CoreDuetD inside of the VM to eliminate services that are known to need high CPU:
```bash
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.coreduetd.osx.plist 
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist
```

## Re-pushing an existing registry tag

On pushing to the registry, a tag is created. It will also be assigned a specific commit ID (not visible to users). Even if you modify the tag locally, such as adding port-forwarding, changes will not be pushed to the registry until you push with a different tag name.

Now, you can simply untag the VM locally and then push it with the same name (after [deleting the VM template]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#delete-template" >}}) or [reverting the tag]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#revert-template-tag" >}})):

> Locally, this does not remove the current STATE of the tag from the VM. Your installed dependencies inside of the VM will remain as long as you don't pull or switch to a different tag.

```bash
anka registry pull -t tag2 TemplateA
anka delete -t tag2 TemplateA
anka modify TemplateA add port-forwarding...
curl ... (delete or revert from registry)
anka registry push -t tag2 TemplateA
```

---

## What's next?

- [Starting and Accessing your VM]({{< relref "intel/Getting Started/starting-and-accessing-your-vm.md" >}})
