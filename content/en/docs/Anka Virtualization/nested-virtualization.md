---
date: 2019-12-12T00:00:00-00:00
title: "Nested Virtualization"
linkTitle: "Nested Virtualization"
weight: 2
description: >
  Running Docker and other types of virtualization within your Anka VMs
---

Starting in Anka 2.5.0, Nested Virtualization has received a large refactor and expanded support. **It is now enabled by default.**

## System Requirements

- At the moment only Big Sur hosts and both Catalina and Big Sur VMs can use Nested Virtualization.

- We've tested on i7 Mac Minis and cannot guarantee that it will work properly on different hardware.

### Supported Technologies

#### Docker
#### Virtualbox
#### Android Emulators
  > [We highly recommend PG is enabled when using Android Emulators]({{< relref "docs/Anka Virtualization/enabling-graphics-acceleration-with-apple-metal.md" >}})

There are however some steps that you need to perform to get Android emulators to run properly:
1. Uninstall previous Anka versions using sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh
2. Ensure Templates/Tags don't have the older nested option enabled:  `anka modify {vmName} set nested 0`
3. When android studio goes to install the intel HAXM extension, you may need to approve the exception in System Preference > Security & Privacy and then reboot (requires a GUI session).
4. You need to ensure that the config.ini for your virtual device/emulator has:
    ```
    hw.gpu.enabled=yes
    hw.gpu.mode=swiftshader_indirect
    ```