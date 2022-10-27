---
title: "Creating your first VM"
linkTitle: "Creating your first VM"
weight: 2
description: >
  Step by step on how to create your first VM
---

{{< hint warning >}}
**This guide, and the commands inside it, must be run as the non-root user on the system. Anka VMs run under a specific user environment. At the moment there is no way to open the Anka Viewer window, which is required for VM preparation, if the VM exists under the root user space.**
{{< /hint >}}

{{< hint warning >}}
It's important to understand that the `anka` CLI, VM creation, modification, etc, is all done from your current user. You can use the [Anka Build Cloud Registry]({{< relref "Anka Build Cloud/_index.md" >}}) to move VMs between users and even hosts.
{{< /hint >}}

{{< hint warning >}}
Starting in Anka 3.1, manual installation of macOS is no longer required. Existing users can expect a similar experience as Intel VM creation.
{{< /hint >}}

{{< hint info >}}
Rosetta can be installed and used inside of the VM.
{{< /hint >}}

## Prerequisites

1. [You've installed the Anka Virtualization package.]({{< relref "apple/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. The host you wish to use for running Anka VMs is using Apple processors **and not Intel.**
3. The host you wish to use has Monterey (>= 12.0) installed.
4. The host you wish to use has networking (requirement of Apple Virtualization.Framework) and access to the internet.

---

## Create your first VM

You have two methods of creating your Anka VMs. We will describe both in this guide, but you only really need to choose one.

1. [With the **anka create** command](#using-the-anka-cli) (recommended).
2. [With the Anka.app UI](#using-the-anka-ui).

---

### Supported VM macOS versions

{{< include file="_partials/apple/Getting Started/_supported-macos-versions.md" >}}

{{< hint warning >}}
**Apple's .app installer files are currently not supported on arm. Instead, you'll need to obtain .ipsw files.**
{{< /hint >}}

---

### Using the Anka CLI

{{< include file="_partials/apple/Anka Virtualization/create/_index.md" >}}

{{< include file="_partials/apple/Anka Virtualization/create/_example.md" >}}

{{< include file="_partials/apple/Anka Virtualization/create/_extra.md" >}}

With `anka create`, we will automatically set up macOS, disable SIP, and enable VNC for you. This is different from the UI creation tool (see below), which still requires manual macOS installation.

#### IPSW vs .app installers

While we recommend you use `anka create -a latest` to automatically download the latest macOS version to install into the VM, you can bring your own IPSW file which is very similar to how Anka 2 works with .app installers.

There are multiple ways to obtain IPSW files. Apple provides these through their `updates.cdn-apple.com` site. You can usually find the official links to the version you want with your preferred search engine. Alternatively, you can use [ipsw.me](https://ipsw.me) to identify and retrieve your desired IPSW file.

---

### Using the Anka UI

1. Click on **Create new VM**.
2. **LEAVE INSTALLER BLANK** and click on Options to set any non-default values you want.
  {{< hint info >}}
  Leaving the installer blank will automatically target the latest macOS version, pulling the IPSW file from the official Apple CDN (`updates.cdn-apple.com`). You can use your own IPSW file with the Anka CLI instead.
  {{< /hint >}}
![installer with pkg]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/ui.png)
3. Be patient while it's creating.

Once the VM is created, you will see it on the sidebar -- Hooray!

![ui with vm in the sidebar list]({{< siteurl >}}images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

#### Start the VM and finish the macOS install

{{< include file="_partials/apple/Anka Virtualization/start/_index.md" >}}

{{< hint info >}}
You can start the VM in Recovery Mode with `ANKA_START_MODE=2`:

```bash
ANKA_START_MODE=2 anka start 12.6
```

{{< /hint >}}


{{< hint warning >}}
**For our addons to install and enable autologin properly, you need to create the VM user as username: `anka` and password: `admin`. If you decide to use your own username and password, you will need to manually enable autologin for the user.**
{{< /hint >}}

1. You’ll need to start the VM with `anka start -uv` to launch the viewer.

  {{< hint warning >}}
  `anka view` does not currently work post-start unless you started it with -v.
  {{< /hint >}}

  {{< hint warning >}}
  `sudo anka view` as a normal user is not possible yet. You'll need to ensure that VNC is enabled to access VMs running under `sudo`.
  {{< /hint >}}

2. Once inside the Anka Viewer/VM, finish the macOS installation **and be sure to install the addons package through the disk we mounted with `-u`**.

![mounted addons]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/addonspkg.png)

3. Reboot the VM.

#### Disable SIP in Recovery Mode (versions <= 3.0.1)

{{< hint warning >}}
SIP is only enabled for VMs created in the Anka App's UI. These instructions are irrelevant for VMs created with `anka create`.
{{< /hint >}}

With SIP enabled, there are two main issues you'll find when running VMs:

1. User or command executions can hang due to a "allowed to access" dialog in the VM's UI. It requires VNC access and manual intervention to get around (no commands to disable this protection feature).
2. Apple's `syspolicyd` will notice applications and processes running for the first time and consume a lot of CPU and RAM trying to scan them.

In order to disable SIP, you need to first launch the VM in recovery mode.

![recovery-mode]({{< siteurl >}}images/apple/getting-started/accessing-your-vm/anka-app-recovery-mode.png)

Then, you launch the Terminal application and execute `csrutil disable`. Once executed and after confirmation that the command worked, you can stop the VM. On next boot, SIP will be disabled.

---

## Optimizing your VM

It's recommended that you disable Spotlight and CoreDuetD inside of the VM to eliminate services that are known to need high CPU:

```bash
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.coreduetd.osx.plist 
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes"
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network"
sudo killall mds || true
sleep 60
sudo mdutil -a -i off /
sudo mdutil -a -i off
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist || true
sudo rm -rf /.Spotlight-V100/*
```

---

## Listing available VMs in the CLI

{{< include file="_partials/apple/Anka Virtualization/list/_example.md" >}}

## Deleting a VM

### Using the Anka UI

![edit menu delete]({{< siteurl >}}images/getting-started/creating-your-first-vm/edit-menu-delete.png)

### Using the Anka CLI

```shell
❯ anka delete test
are you sure you want to delete vm 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 test [y/N]:
```

---

## VM Clones

### Disk Optimization

Customers coming from Anka 2 will know that when you clone an _untagged_ VM, it will share the underlying VM image files between the two. However, this is not the case for Anka 3. As of right now, sharing of the underlying VM image files between a clone and its source requires first creating a tag for the source _before_ you clone. You can do this with `anka push --local`, or just a regular `anka push` if you've got a running [Anka Build Cloud Registry]({{< relref "Anka Build Cloud/_index.md" >}}).

{{< hint info >}}
Don't worry, clones will not have access to change the original source VM.
{{< /hint >}}

{{< include file="_partials/apple/Anka Virtualization/push/_index.md" >}}

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

_The above example shows the tag "vanilla" does not exist locally until we execute the `anka push --local`._

{{< hint info >}}
Cloned VMs will use a trivial amount of disk space until you start them. Once started, an empty image is created and connected on top of existing images and any changes to or in macOS are then added to it.
{{< /hint >}}

{{< hint info >}}
To switch between tags locally, you can use the `anka pull --local --tag {targetTagname} {VMName}` command:

{{< include file="_partials/apple/Anka Virtualization/pull/_index.md" >}}

{{< /hint >}}

You can easily create VM clones from a source VM and _its current state_ using `anka clone`:

{{< include file="_partials/apple/Anka Virtualization/clone/_index.md" >}}

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

### VM Templates

Once a VM has been tagged, it becomes a "VM Template". The VM template+tag's state cannot be permanently modified unless you create a new tag, post-changes. This is very reminiscent of how `git commit` works. You can execute comands and modify the state of the VM after tagging it, but it will not save the changes to the existing template+tag. This is important to consider when using the [Anka Build Cloud Registry]({{< relref "Anka Build Cloud/_index.md" >}}) since it will only push the state of the VM when the tag was created, not after.

In summary, when cloning a tagged VM you have two options:

1. Clone from the current VM state, regardless of the state when it was tagged (`anka clone {source} {clone}`).
2. Clone the state of a VM template/tag by targetting the tag by name (`anka clone --tag {tagName} {source} {clone}`), regardless of what has been done to it since tagging.

{{< hint info >}}
The clone is not automatically tagged.
{{< /hint >}}


```bash
❯ anka list | grep test
| test (v1)                                 | ff06aa5b-0825-4f86-b5d0-c1cdb39fcedf | Jan 25 13:15:10 2022 | stopped |

❯ anka clone --tag v1 test test3                  
8a4e0033-29b4-4c29-8a0c-51fa53093d1c

❯ anka list | grep test         
| test3                                     | 8a4e0033-29b4-4c29-8a0c-51fa53093d1c | Feb 3 12:01:34 2022  | stopped |
| test (v1)                                 | ff06aa5b-0825-4f86-b5d0-c1cdb39fcedf | Jan 25 13:15:10 2022 | stopped |
```

---

## What's next?

- [Acessing your VM]({{< relref "apple/Getting Started/accessing-your-vm.md" >}})
