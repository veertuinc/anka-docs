

---
date: 2019-12-12T00:00:00-00:00
title: "Configure"
linkTitle: "Configure"
weight: 5
description: >
  How to configure Anka CLI
---


## View configuration parameters

View all default configuration settings for Anka installation on the host with `anka config -l` command.

```shell
anka config --help
Usage: anka config [OPTIONS] [PARAM]...

  Manage Anka configuration

Options:
  -l, --list   List value parameter(s)
  -r, --reset  Reset value of parameter(s)
  --help       Show this message and exit.

```

```shell
anka config -l
+-----------------------------+-----------------------------------------------------------------------------+
| registry_remotes_file_path  | /Users/XXX/.anka/remote                                            |
| mac_random_bytes            | 2                                                                           |
| disk_eraser_tool            | /Library/Application Support/Veertu/Anka/tools/erase_disk_image.sh          |
| process_type                | Interactive                                                                 |
| default_passwd              | admin                                                                       |
| nvram_path                  | /Library/Application Support/Veertu/Anka/uefi/anka_vars.fd                  |
| vm_lib_dir                  | /Users/XXX/Library/Application Support/Veertu/Anka/vm_lib/         |
| anka_image_maker_executable | /Library/Application Support/Veertu/Anka/bin/anka_image                     |
| mac_prefix_file             | /Users/XXX/.anka/mac_prefix.txt                                    |
| options_api_url             | https://8ewgf8mtn4.execute-api.us-west-2.amazonaws.com/prod/key/info        |
| addons_pkg                  | com.veertu.anka.guestaddons.pkg                                             |
| log_dir                     | /Users/XXX/Library/Logs/Anka/                                      |
| addons_postupdate           | /Library/Application Support/Veertu/Anka/tools/addons_postupdate.sh         |
| vnc_password                | admin                                                                       |
| nice                        | 0                                                                           |
| product_root                | /Library/Application Support/Veertu/Anka                                    |
| addons_disk_tool            | /Library/Application Support/Veertu/Anka/tools/addons_create_update_disk.sh |
| time_sync                   | 1                                                                           |
| max_vms_allowed             | 255                                                                         |
| portfwd_base                | 10000                                                                       |
| operations_timeout          | 300                                                                         |
| bios_path                   | /Library/Application Support/Veertu/Anka/uefi/anka_code.fd                  |
| table_fmt                   | psql                                                                        |
| anka_executable             | /Library/Application Support/Veertu/Anka/bin/ankahv                         |
| block_nocache               | False                                                                       |
| default_user                | anka                                                                        |
| vm_lock_dir                 | /Users/XXX/Library/Application Support/Veertu/Anka/vm_lib/.locks   |
| state_lib_dir               | /Users/XXX/Library/Application Support/Veertu/Anka/state_lib/      |
| mac_counter                 | /Users/XXX/.anka/mac_counter.txt                                   |
| uhost_executable            | /Library/Application Support/Veertu/Anka/bin/uhost                          |
| addons_identifier           | com.veertu.anka.addons.core.pkg                                             |
| addons_image                | /Library/Application Support/Veertu/Anka/guestaddons/anka-addons-mac.iso    |
| ca_bundle                   | /Users/XXX/.anka/ca-bundle.crt                                     |
| permanent_mac_prefix        | 86,69,69                                                                    |
| anka_viewer                 | /Library/Application Support/Veertu/Anka/bin/AnkaView.app                   |
| img_lib_dir                 | /Users/XXX/Library/Application Support/Veertu/Anka/img_lib/        |
| puller_threads              | 4                                                                           |
| allow_nonunique_names       | False                                                                       |
| log_file                    | anka.log                                                                    |
| adresses_per_byte           | 250                                                                         |
| trim_disk                   | True                                                                        |
+-----------------------------+-----------------------------------------------------------------------------+
```

## View specific configuration parameter
See value for a specific configuration parameter with `anka config $PARAM_NAME`.


## Changing default storage

* Default storage location for Anka VMs is at:
1) `vm_lib_dir - /Users/XXX/Library/Application Support/Veertu/Anka/vm_lib/`
2) `state_lib_dir - /Users/XXX/Library/Application Support/Veertu/Anka/state_lib/` 
3) `img_lib_dir - /Users/XXX/Library/Application Support/Veertu/Anka/img_lib/`.

* Log directory is at `log_dir| /Users/XXX/Library/Logs/Anka/` and log file is `anka.log`.

### Managing Anka VMs on external storage on the host
If the host machines on which you install Anka has insufficient storage for Anka VMs, you can point Anka to use the external storage for the images.

There are three configuration parameters to control location for storing Anka VMs.
Assuming you want your Anka VMs to be on **"/Volumes/ExternalDrive/"**, perform these steps:

1. `anka config img_lib_dir /Volumes/ExternalDrive/image_lib`
2. `anka config state_lib_dir /Volumes/ExternalDrive/state_lib`
3. `anka config vm_lib_dir /Volumes/ExternalDrive/vm_lib`


There are different settings for every user, plus root (each user has it's own set of configuration parameters and VM directory). 
Using sudo changes the root's settings.


