---
date: 2022-10-13T01:00:00-00:00
title: "Anka Virtualization 3.1.0 (apple/arm64)"
---

We are very excited to announce Anka Virtualization 3.1. In this version, you're going to find several important features that all of our users will benefit from. Here is a summary:

1. [MacOS installation through the `anka create` command is now automated.](#macos-installation-through-the-anka-create-command-is-now-automated)
    - SIP is now disabled by default inside of the VM.
    - VNC is now enabled by default inside of the VM.
2. The `anka create` command will now produce `stopped` state VMs. (suspending is not supported by Apple yet)
3. [Shorter `anka modify` and `port-forwarding` command.](#shorted-anka-modify-and-port-forwarding-command)
4. `ANKA_LOG_LEVEL="debug"` is available as a replacement for `anka --debug`.
5. [Support for the Anka Build Cloud's UAKs when interacting with the registry commands.](#support-for-the-anka-build-clouds-uaks-when-interacting-with-the-registry-commands)
6. [Ability to resize the VM's disk.](#ability-to-resize-the-vms-disk)
7. [Targeting of specific macOS versions with `anka create -a`](#targeting-of-specific-macos-versions-with-anka-create--a)

---

## MacOS installation through the `anka create` command is now automated

Anka 3.0.x began Anka's support for Apple Silicon / M1 / ARM virtualization. However, it was significantly different from the Anka 2/Intel version which produced a VM, using `anka create`, with macOS installed, the user set up, and SIP disabled. Users had to manually install macOS and perform any other necessary steps before their VM was functional.

Starting in Anka 3.1, we're the first to solve this problem by automating the macOS installation, setup, and disabling of SIP. We even automatically enable VNC so users can work around the limitations in Anka View (doesn't work under `sudo`). We're excited about this release and look forward to it improving your production environments running Apple Silicon VMs.


## Shorter `anka-modify` and `port-forwarding` command

{{< include file="_partials/apple/Anka Virtualization/modify/port/_example.md" >}}

## Support for the Anka Build Cloud's UAKs when interacting with the registry commands

[Read more about the Anka Build Cloud Advanced Security Token Feature here.]({{< relref "Anka Build Cloud/Advanced Security Features/token-authentication.md" >}})

{{< include file="_partials/apple/Anka Virtualization/registry/_index.md" >}}

## Ability to resize the VM's disk

{{< include file="_partials/apple/Anka Virtualization/modify/disk/_example.md" >}}

## Targeting of specific macOS versions with `anka create -a`

```bash
❯ anka create --list
+---------+--------+
| version | build  |
+---------+--------+
| 12.6    | 21G115 |
+---------+--------+

❯ anka create -a 12.6 12.6-arm64
 14% [||||||||                                                    ] 10:02 ETA

❯ anka create -a latest 12.6-arm64
```