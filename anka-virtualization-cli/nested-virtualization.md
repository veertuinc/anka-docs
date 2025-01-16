---
date: 2019-12-12T00:00:00-00:00
title: "Nested Virtualization (intel only)"
weight: 8
description: >
  Running Docker and other types of virtualization within your Anka VMs.
---

Starting in Anka 2.5.0, Nested Virtualization has received a large refactor and expanded support. **It is now enabled by default.**

{{< hint warning >}}
Please avoid suspending VMs while Docker or other nested virtualization apps are started. You will need to start the apps post-vm start.
{{< /hint >}}

## System Requirements

- Host macOS >= Catalina + Anka VM with Big Sur.

- We've tested on i7 Mac Minis and cannot guarantee that it will work properly on different hardware.

### Supported Technologies

#### QEMU

- Fully functional as of 3.5.2.

#### Docker

- Fully functional as of 3.5.2.

{{< hint warning >}}
Please avoid suspending VMs while Docker or other nested virtualization apps are started. You will need to start the apps post-vm start.
{{< /hint >}}

#### Virtualbox

- Fully functional as of 3.5.2.

#### Android Emulators

{{< hint info >}}
[We highly recommend PG is enabled when using Android Emulators]({{< relref "anka-virtualization-cli/graphics-acceleration-apple-metal.md" >}})
{{< /hint >}}

{{< hint warn >}}
Android emulators are very slow inside of VMs. Expect long delays (like 1+ minutes waiting at a black screen) booting the emulator. Also, you will receive lots of "isn't responding" warnings in the UI which may impact your tests. We recommend you avoid using Android Emulators on MacOS VMs.
{{< /hint >}}

There are however some steps that you need to perform to get Android emulators to run properly:

1. When android studio goes to install the intel HAXM extension (you may need to manually install it [with these instructions](https://github.com/intel/haxm/wiki/Installation-Instructions-on-macOS)), you need to approve the exception in System Preference > Security & Privacy and then reboot (requires a GUI session).
2. You need to ensure that the config.ini for your virtual device/emulator has:

    ```shell
    hw.cpu.ncore=5
    hw.gpu.enabled=yes
    hw.gpu.mode=swiftshader_indirect
    ```