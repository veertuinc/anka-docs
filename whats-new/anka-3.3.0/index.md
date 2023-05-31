---
date: 2023-04-04T01:00:00-00:00
title: "Anka Virtualization 3.3.0"
---

We are very excited to announce Anka Virtualization 3.3.0. In this version, you're going to find several important features that all of our users will benefit from. Here is a summary:

1. [Combined ARM and Intel PKG Installers](#combined-arm-and-intel-pkg-installers)
1. [VM Networking IP Filtering](#vm-networking-ip-filtering)
1. [Automated log in for autologin disabled VM](#automated-log-in-for-autologin-disabled-vms)
1. [Support for FileVault (ARM)](#support-for-filevault-arm)
1. [Anka click scripts inside VM (ARM)](#anka-click-scripts-inside-vm-arm)

---

## Combined ARM and Intel PKG Installers

Customers will now find a single PKG installer for both Intel and ARM. Existing download URLs will remain the same, however, you will no longer be able to rely on the `-intel` and `-arm` suffixes on the PKG file.

## VM Networking IP Filtering

{{< include file="_partials/anka-virtualization-cli/advanced-security-features/ip-filtering.md" >}}

## Automated log in for autologin disabled VMs

Users requiring that VMs do not have autologin enabled can now set `anka modify {VM_NAME_HERE} set custom-variable login_passwd {PASSWORD_HERE}` with the appropriate password for the VM and allow Anka, post-boot, to run an [anka click script](https://github.com/veertuinc/anka-click-scripts) that logs the user in. You can also specify `anka start --login-passwd "${VM_PASSWD}" "${VM_NAME}"`.

## Support for FileVault (ARM)

ARM users can now enable FileVault inside of their VMs. However, keep in mind that Apple disables autologin while FileVault is enabled. This will break existing flows until users update their `sudo anka config default_passwd` so that our [anka click script](https://github.com/veertuinc/anka-click-scripts) can perform the log in, post-boot.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/start/_index.md" >}}

## Anka click scripts inside VM (ARM)

ARM users will be able to use `/Library/Application\ Support/Veertu/Anka/bin/click` inside of their VMs to run click scripts. This is a major improvement for automation. An example is using [the Prefer Discrete GPU in iOS simulator click script](https://github.com/veertuinc/anka-click-scripts/blob/main/13.0/simulator-prefer-discrete-gpu/simulator-prefer-discrete-gpu.click) to improve iOS simulator performance.
