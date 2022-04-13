---
date: 2019-12-12T00:00:00-00:00
title: "Environment Variables"
linkTitle: "Environment Variables"
weight: 4
description: >
  A list of available environment variables for the Anka CLI 
---

By default, all `anka config` options are available as Environment Variables by simply converting their name to uppercase and prefixing it with `ANKA_`. So, `default_user` becomes `ANKA_DEFAULT_USER` **and the ENV takes precedence**.

Outside of the config environment variables, there are a few others you might find useful:

| Name | Description |
| --- | --- |
| ANKA_CREATE_SUSPEND             | 0 produces VM in stopped state on `anka create` |
| ANKA_UPDATE_SUSPEND             | 0 do not suspend VM on `anka start -u` |
| ANKA_HOSTNAME                   | allows to specify VM host name on VM start |
| ANKA_BLOCK_SIZE                 | ANKA image block size (1MB default) |
| ANKA_GPU_LOCATION               | allows to specify GPU location on VM start |
| ANKA_GPU_NAME                   | allows to specify GPU name on VM start |
| ANKA_CHECK_CONSISTENCY          | 0 turns off file system integrity checks |
| ANKA_CHECK_FREE_SPACE           | 0 turns off free space validation on pull |
| ANKA_CLAIM_NAME                 | implicit claim name in the `usb claim` command |
| ANKA_DELETE_IGNORE_ABSENT       | 1 to not fail `anka delete {VM}` if VM is missing |
| ANKA_DELETE_LOGS                | 0 do not delete VM logs after VM deletion |
| ANKA_DISK_CONTROLLER            | allows to specify disk controller on `anka create` |
| ANKA_FORCE_ABORT                | 1 kills VM with SIGABRT on `anka stop -f` |
| ANKA_IMG_PATH                   | allows to override igm_lib_dir and state_lib_dir search paths |
| ANKA_MACHINE_READABLE           | same as `--machine-readable` |
| ANKA_NETWORK_CONTROLLER         | allows to specify network controller on `anka create` |
| ANKA_NETWORK_MODE               | allows to specify network mode on `anka create` |
| ANKA_NO_ADDONS                  | 1 do not create addons USB device |
| ANKA_NO_RUN                     | 1 do not create ankarun USB device |
| ANKA_NO_SHAREDFS                | 1 do not create ankacp USB device |
| ANKA_NO_VIEW                    | 1 do not create anka view USB device |
| ANKA_USER                       | specifies user for policy affected commands |
| ANKA_PASSWORD                   | specify password for policy operations |
| ANKA_PG_BIOS                    | path to the PG video BIOS |
| ANKA_PREEMPTION_RATE            | allows to control preempt parameter on `anka create` |
| ANKA_PROPAGATE_LICENSE          | 1 makes Anka license visible inside VM guest |
| ANKA_RFB_FPS                    | specify refresh rate of VNC (25Hz by default) |
| ANKA_SHAREDFS_ATTACH            | 0 do not attach ankacp USB device on VM start |
| ANKA_SUSPEND_AFTERBOOT_INTERVAL | specify interval between first boot (after VM create) and suspend (5 min default) |
| ANKA_SUSPEND_TIMEOUT            | configures minimum VM uptime before suspend |
| ANKA_UHOST_RESETCONFIGURATION   | 0 do issue SET_CONFIGURATION to the usb device if it's also configured to the same number |
| ANKA_UHOST_RESETTIMEOUT         | wait after USB reset to continue |
| ANKA_VIEW_ATTACH                | 0 do not attach anka view USB device initially |
| ANKA_VLAN                       | specifies network VID on VM start |
| ANKA_ADDONS_ATTACH              | 0 means do not attach addonsd to the hypervisor (graceful shutdown, timesync won't work)) |
| ANKA_BLOCK_NOCACHE              | 1 turns on the F_NOCACHE flag (default) |
| ANKA_BLOCK_NODIRECT             | 0 turns off the F_NODIRECT flag |
| ANKA_TIME_SYNC                  | 0 turns off the built in time sync for Apple's hypervisor |
