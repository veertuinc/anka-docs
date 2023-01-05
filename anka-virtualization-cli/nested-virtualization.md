---
date: 2019-12-12T00:00:00-00:00
title: "Nested Virtualization (intel only)"
weight: 8
description: >
  Running Docker and other types of virtualization within your Anka VMs.
---

Starting in Anka 2.5.0, Nested Virtualization has received a large refactor and expanded support. **It is now enabled by default.**

## System Requirements

- At the moment only Big Sur hosts and both Catalina and Big Sur VMs can use Nested Virtualization.

- We've tested on i7 Mac Minis and cannot guarantee that it will work properly on different hardware.

### Supported Technologies

#### Docker

{{< hint warning >}}
Please avoid suspending VMs while Docker or other nested virtualization apps are started. You will need to start the apps post-vm start.
{{< /hint >}}

#### Virtualbox

{{< hint warning >}}
You must set a single vCPU for the virtualbox VM for it to run properly.
{{< /hint >}}

#### Android Emulators

{{< hint info >}}
[We highly recommend PG is enabled when using Android Emulators]({{< relref "anka-virtualization-cli/graphics-acceleration-apple-metal.md" >}})
{{< /hint >}}

There are however some steps that you need to perform to get Android emulators to run properly:

1. Uninstall previous Anka versions using `sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`
2. When android studio goes to install the intel HAXM extension, you may need to approve the exception in System Preference > Security & Privacy and then reboot (requires a GUI session).
3. You need to ensure that the config.ini for your virtual device/emulator has:

    ```shell
    hw.gpu.enabled=yes
    hw.gpu.mode=swiftshader_indirect
    ```
