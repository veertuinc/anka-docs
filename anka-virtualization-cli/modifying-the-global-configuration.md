---
date: 2019-12-12T00:00:00-00:00
title: "Modifying the Global Configuration"
linkTitle: "Modifying the Global Configuration"
weight: 4
description: >
  How to modify the Global Anka Virtualization configuration
---

{{< include file="_partials/anka-virtualization-cli/command-line-reference/config/_index.md" >}}

## Viewing the configuration

View all default configuration settings for Anka installation on the host with `anka config` command:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/config/_example.md" >}}

> You can see a value for a specific configuration parameter with `anka config $PARAM_NAME`.

- The default VM username is `anka`
- The default VM password is `admin`
- The default storage locations for Anka VM Templates & Tags are:
  - vm_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/vm_lib/`
  - state_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/state_lib/`
  - img_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/img_lib/`
- The log directory is `/Users/XXX/Library/Logs/Anka/` and log file is `anka.log`.

## Changing the default Anka VM storage location

Depending on how many Anka VMs you have, the disk usage might be too much for the default storage location. There are three configuration parameters to control location for storing Anka VMs.

{{< hint warning >}}
Anka 3.x and greater requires a APFS volume.
{{< /hint >}}

{{< hint info >}}
It's recommended to keep the vm_lib_dir on the local disk as it contains file locks.
{{< /hint >}}

Assuming you want to store your Templates on **/Volumes/ExternalDrive/**, perform these steps:

1. `anka config img_lib_dir /Volumes/ExternalDrive/img_lib`
2. `anka config state_lib_dir /Volumes/ExternalDrive/state_lib`
3. `anka config vm_lib_dir /Volumes/ExternalDrive/vm_lib`
4. Go under System Preferences > Security & Privacy > Full Disk Access and add `/Library/Application\ Support/Veertu/Anka/bin/ankahv` as well as `/Library/Application\ Support/Veertu/Anka/bin/anka_agent`. Make sure the slider button is enabled too!
