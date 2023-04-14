---
date: 2023-04-04T01:00:00-00:00
title: "Anka Virtualization 3.3.0"
---

We are very excited to announce Anka Virtualization 3.3.0. In this version, you're going to find several important features that all of our users will benefit from. Here is a summary:

1. [Combined ARM and Intel PKG Installers](#combined-arm-and-intel-pkg-installers)
1. [VM Networking IP Filtering](#vm-networking-ip-filtering)
1. [Automated log in for autologin disabled VM]()
1. [Support for FileVault (ARM)](#support-for-filevault-arm)

---

## Combined ARM and Intel PKG Installers

Customers will now find a single PKG installer for both Intel and ARM. Existing download URLs will remain the same, however, you will no longer be able to rely on the `-intel` and `-arm` suffixes on the PKG file.

## VM Networking IP Filtering

{{< include file="_partials/anka-virtualization-cli/advanced-security-features/ip-filtering.md" >}}

## Automated log in for autologin disabled VM

Users requiring that VMs do not have autologin enabled can now set `sudo anka config default_passwd` with the appropriate password for the VM and allow Anka, post-boot, to run an [anka click script](https://github.com/veertuinc/anka-click-scripts) that logs the user in. You can also specify `anka start --login-passwd "${VM_PASSWD}" "${VM_NAME}"`.

## Support for FileVault (ARM)

ARM users can now enable FileVault inside of their VMs. However, keep in mind that Apple disables autologin while FileVault is enabled. This will break existing flows until users update their `sudo anka config default_passwd` so that our [anka click script](https://github.com/veertuinc/anka-click-scripts) can perform the log in, post-boot.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/start/_index.md" >}}
