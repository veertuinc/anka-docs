
---
date: 2019-07-03T22:24:47-05:00
title: "Setup"
linkTitle: "Setup"
weight: 2
description: >
  Setting up Anka Flow, How to install, uninstall and upgrade Anka Flow.
---



## Installation and Configuration
Anka when activated with the Anka Flow license enables installation of Anka package on the developer mac machines. It enables the developers to create and work inside Anka macOS VMs on their local machines. Developers can use the [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) interface to work inside Anka VMs in a container like manner.  

You can also use Anka Flow in combination with Anka Build and enable your developers to pull the build environments from the central registry on their local machines.

### Install

Install Anka package on the mac developer machine. ***Note*** Anka Flow is only supported on MacBook models and iMac.  

Installing the Anka Hypervisor binary is remarkably simple, just download it from [Anka Flow Download page](https://veertu.com/download-anka-run/).  
Run the `anka.pkg` installer. The Anka package includes the Core hypervisor and `anka` guest Addons.

***Note*** In order to create VMs with nested feature enabled, select nested virtualization checkbox in Customizations while installing Anka through the GUI method.  
If using command line, use the following command `sudo installer -applyChoiceChangesXML nanka.xml -pkg Ankaxx.pkg -target /`.

After running the installer package, `anka` is found at `/usr/local/bin/anka`.

Verify the installation by running [`anka version`]({{< relref "docs/Anka CLI/command-reference.md#version" >}}) command.

After install, if the users want to, they can change the location of this default Anka install location link manually.

Execute `sudo mv /usr/local/bin/anka /to/any/other/location/`.

Verify the installation by running [`anka version`]({{< relref "docs/Anka CLI/command-reference.md#version" >}}) command.

View all settings for Anka installation on the host with `anka config -l` command.

Activate with your Anka Flow license key.  

```
anka license activate <key>
```

### Upgrading the Anka Package
1. Download the latest version of Anka package (.pkg) and stop all running VMs.
2. Before upgrading, you will also need to force stop any suspended VMs with the `anka stop -f {template}` command.

***Note*** For major Anka releases, it maybe required to upgrade guest addons in existing Anka VMs. Check the release notes to identify if this step is required or not.

3. Upgrade guest addons on the VM. Execute the following command to upgrade guest addons in existing VMs to the current release.

```
anka stop -f {template}
anka start -u {template}
Preparing update configuration
Installing updates
Suspending
update succeeded
```
If you are not able to upgrade the guest addons tool using the `anka start -u {template}` command, then you have a very old version of guest addon tools on your VM. You will first need to manually update them. Contact Veertu support.
4. Push your upgraded VMs to the Registry.

### Uninstall
From the command line, execute the following command. Make sure all your VMs are stopped.

`sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`

