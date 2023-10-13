---
title: "Creating VMs"
linkTitle: "Creating VMs"
weight: 2
description: >
  Step by step on how to create VMs
---

{{< hint warning >}}
It's important to understand that the `anka` CLI VM creation, modification, etc, is all exclusively within your current user. The root user and non-root users will have different environments. You can use the [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}) to move VMs between users (and hosts).
{{< /hint >}}

## Prerequisites

1. [You've installed the Anka Virtualization package.]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
2. The host you wish to use has networking (requirement of Apple Virtualization.Framework) and access to the internet.

---

## Create your first VM

You have two methods of creating your Anka VMs. We will describe both in this guide, but you only really need to choose one.

1. [With the **anka create** command](#using-anka-create) (recommended).
2. [With the Anka.app UI](#using-the-anka-gui).

---

### Supported VM macOS versions

Anka allows you to create VMs for the following macOS versions:

{{< rawhtml >}}
<div style="display:flex;">
<table>
<tbody style="text-align:center">
  <tr>
    <td style="font-size: 1.5rem; background-color: #f2e6ff;"><b>INTEL</b></td>
    <td style="background-color: #f2e6ff;"><b>Anka 2.5.7</b></td>
    <td style="background-color: #f2e6ff;"><b>Anka 3.3.x</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 10.14</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 10.15</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 11.x</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.x</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.x<a href="https://support.apple.com/en-us/HT213264">*</a></b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.x</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
</tbody>
</table>
<table>
<tbody style="text-align:center;">
  <tr>
    <td style="font-size: 1.4rem; background-color: #ccebff;"><b>Apple/ARM</b></td>
    <td style="background-color: #ccebff;"><b>Anka 3.3.x</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;<small>**</small></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;<small>**</small></td>
  </tr>
</tbody>
</table>
</div>
{{< /rawhtml >}}


{{< hint warning >}}
**(*) Apple has limited the ability to install Ventura to specific hardware models. <a href="https://support.apple.com/en-us/HT213264">You can view a list of supported models here.</a>**
{{< /hint >}}

{{< hint warning >}}
**(\*\*) ARM USERS: Apple is requiring the host OS be 13.x for creating and running VMs with macOS >= 13.3.</a>**
{{< /hint >}}

{{< hint warning >}}
**ARM USERS:** Creating macOS 14.x or higher VMs requires that Xcode >= 15.0 OR the Device Support package is installed on the host (not in the VM). Alternatively, you can install the Device Support Image/dmg Apple provides. This is a requirement from Apple at the moment.

There is also a rare problem where your Xcode is not fully set up and still creates problems. Be sure to run the following:

```bash
sudo xcodebuild -license accept
sudo xcodebuild -runFirstLaunch
for PKG in $(/bin/ls /Applications/Xcode.app/Contents/Resources/Packages/*.pkg); do
    sudo /usr/sbin/installer -pkg "$PKG" -target /
done
```
{{< /hint >}}

{{< hint warning >}}
**ARM USERS:** Creating macOS 14.x or higher VMs requires that Xcode >= 15.0. Alternatively, you can install the Device Support Image/dmg Apple provides. This is a requirement from Apple at the moment.
{{< /hint >}}

{{< hint warning >}}
**Apple's .app installer files are currently not supported on ARM. Instead, you'll need to obtain .ipsw files.**
{{< /hint >}}

---

### Using `anka create`

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_example.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_extra.md" >}}

After executing `anka create`, Anka will automatically set up macOS, create the user `anka` with password: `admin`, disable SIP, and enable VNC for you. The VM will then be stopped.

---

### Using the Anka GUI

1. Click on **Create new VM**.
2. **LEAVE INSTALLER BLANK** and click on Options to set any non-default values you want.
  {{< hint info >}}
  Leaving the installer blank will automatically target the latest macOS version, pulling the IPSW file from the official Apple CDN (`updates.cdn-apple.com`). You can use your own IPSW file with the Anka CLI instead.
  {{< /hint >}}
![installer with pkg]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/ui.png)
3. Be patient while it's creating.

Once the VM is created, you will see it on the sidebar -- Hooray!

![ui with vm in the sidebar list]({{< siteurl >}}images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

#### Set up the VM (post-GUI creation)

{{< hint warning >}}
This is not needed if you ran `anka create`.
{{< /hint >}}

The GUI tool will not automatically set up macOS and requires you to perform several steps manually.

1. Start the VM with `anka start -uv` to launch the [Anka Viewer]({{< relref "anka-virtualization-cli/command-line-reference.md#view" >}}) and addons. **Please do not use `-uv` in your scripts when starting VMs. It is only for interactive/logged in sessions.**

- `anka view` does not currently work post-start unless you started it with -v.
- **ARM USERS:** `sudo anka view` as a normal user is not possible yet. You'll need to ensure that VNC is enabled to access VMs running under `sudo`.

2. Once inside the Anka Viewer/VM, finish the macOS installation **and be sure to install the addons package through the disk we mounted with `-u`**.

3. After you're finished, reboot the VM.

![mounted addons]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/addonspkg.png)

{{< hint warning >}}
**For our addons to install and enable autologin properly, you need to create the VM user as username: `anka` and password: `admin`. If you decide to use your own username and password, you will need to manually enable autologin for the user.**
{{< /hint >}}

#### Disable SIP in Recovery Mode (post-GUI creation)

{{< hint info >}}
You can start the VM in Recovery Mode with `ANKA_START_MODE=2`:

```bash
ANKA_START_MODE=2 anka start 12.6
```

{{< /hint >}}

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

It's recommended that you disable:

- Spotlight and `coreduetd`:

```bash
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.coreduetd.osx.plist || true
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes" || true
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network" || true
sudo killall mds || true
sleep 60
sudo mdutil -a -i off / || true
sudo mdutil -a -i off || true
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist || true
sudo rm -rf /.Spotlight-V100/*
```

- SIP, as `syspolicyd` will scan running processes and slow everything down.

---

## Listing available VMs in the CLI

{{< include file="_partials/anka-virtualization-cli/list/_example.md" >}}


## Stop or Suspend the VM

{{< hint warning >}}
Suspending VMs is only possible on intel currently.
{{< /hint >}}

{{< hint info >}}
This is not necessary for `anka modify` commands.
{{< /hint >}}

Once you've finalized your changes inside of the VM, be sure to use `anka stop` or `anka suspend`.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/stop/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/suspend/_index.md" >}}

## Deleting a VM

### Anka CLI

```shell
❯ anka delete test
are you sure you want to delete vm 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 test [y/N]:
```

### Anka GUI

![edit menu delete]({{< siteurl >}}images/getting-started/creating-your-first-vm/edit-menu-delete.png)

---

## VM Clones

### Disk Optimization

Customers coming from Anka 2 will know that when you clone a VM (_untagged_ or _tagged_), it will share the underlying VM image files between the two. However, this is not the case for Anka 3. As of right now, sharing of the underlying VM image files between a clone and its source requires first creating a tag for the source _before_ you clone. You can do this with `anka push --local`, or just a regular `anka push` if you're running the [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}). Don't worry, clones will not have access to change the original source VM state.

{{< include file="_partials/anka-virtualization-cli/push/_index.md" >}}

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

{{< include file="_partials/anka-virtualization-cli/command-line-reference/pull/_index.md" >}}

{{< /hint >}}

### Cloning

You can easily create VM clones from a source VM and _its current state_ using `anka clone`:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/clone/_index.md" >}}

{{< hint info >}}
Don’t worry, at no point do clones have access to change the original source VM state.
{{< /hint >}}

{{< hint info >}}
All clones share as many underlying layers and data as possible.
{{< /hint >}}

There are two types of cloning you can perform:

1. **Shallow Clone**: `anka clone {source} {dest}` -- Shallow clones allow you to get a distinct entity, separate from the source, with a new name and UUID.
2. **Full Clone**: `anka clone --copy {source} {dest}` -- Full clones create a copy that merges all underlying layers so that they cannot be shared with other VMs. While this could theoretically shrink the size of the VM, it loses the ability to re-use existing layers from other VMs on the host and can actually use more disk space than before.

```bash
❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+


❯ anka clone 12.0.1 12.0.1-xcode13
6070ee59-6c16-4c93-ba7a-122b66b1472a

❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1-xcode13   | 6070ee59-6c16-4c93-ba7a-122b66b1472a | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
```

{{< hint info >}}
**Note for Intel Users**
`anka show` output will indicate the logical size (what you set with `anka create --disk` or with `anka modify {vm} disk`) and also "on disk" usage. The "on disk" usage is a combination of all shared data and not unique usage for itself if the VM is cloned from another. Unique usage can be obtained using `anka run {vmname} bash -c "df -h"`.
{{< /hint >}}

### VM Templates

{{< include file="_partials/anka-build-cloud/vm-templates.md" >}}

## Anka Build Cloud

### Add the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

### (Anka Build Cloud only) Push the VM to the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

---

## What's next?

- [Acessing your VM]({{< relref "anka-virtualization-cli/getting-started/accessing-your-vm.md" >}})
