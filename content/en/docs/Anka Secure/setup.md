
---
title: "Setup"
linkTitle: "Setup"
weight: 3
date: 2019-12-12
description: >
  How to install, uninstall and upgrade Anka Secure.
---

## Installation and Configuration
Anka when activated with the Anka Secure license enables installation of Anka package all mac hardware with Secure modules. It enables administrators to create policies for the VM and assign users/groups to the VM policy.

Installing the Anka Hypervisor binary is remarkably simple, just download it from [Anka Secure Download page](https://veertu.com/download-anka-secure/).  
Run the `anka.pkg` installer. The Anka package includes the Core hypervisor and `anka` guest Add-ons.

After running the installer package, `anka` is found at `/usr/local/bin/anka`.

Verify the installation by running [`anka version`]({{< relref "docs/Anka CLI/command-reference.md#version" >}}) command.

After install, if the users want to, they can change the location of this default Anka install location link manually.

Execute `sudo mv /usr/local/bin/anka /to/any/other/location/`.

Verify the installation by running [`anka version`]({{< relref "docs/Anka CLI/command-reference.md#version" >}}) command.

View all settings for Anka installation on the host with `anka config -l` command.

Activate with your Anka Secure license key.  
```
sudo anka license activate <key>
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
If you are not able to upgrade the guest add-ons tool using the `anka start -u {template}` command, then you have a very old version of guest addon tools on your VM. You will first need to manually update them. Contact Veertu support.
4. Push your upgraded VMs to the Registry.

### Uninstall
From the command line, execute the following command. Make sure all your VMs are stopped.

`sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`
