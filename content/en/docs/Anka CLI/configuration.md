---
date: 2019-12-12T00:00:00-00:00
title: "Configuration"
linkTitle: "Configuration"
weight: 2
description: >
  How to configure the Anka CLI
---

{{< include file="shared/content/en/docs/Anka CLI/partials/config/_index.md" >}}

## Viewing the configuration

View all default configuration settings for Anka installation on the host with `anka config -l` command:

{{< highlight shell "hl_lines=13 27 31 57" >}}
❯ anka config -l
+-----------------------------+-----------------------------------------------------------------------------+
| registry_remotes_file_path  | /Users/nathanpierce/.anka/remote                                            |
+-----------------------------+-----------------------------------------------------------------------------+
| disk_eraser_tool            | /Library/Application Support/Veertu/Anka/tools/erase_disk_image.sh          |
+-----------------------------+-----------------------------------------------------------------------------+
| mitigations                 | 0                                                                           |
+-----------------------------+-----------------------------------------------------------------------------+
| options_api_url             | https://8ewgf8mtn4.execute-api.us-west-2.amazonaws.com/prod/key/info        |
+-----------------------------+-----------------------------------------------------------------------------+
| propagate_license           | False                                                                       |
+-----------------------------+-----------------------------------------------------------------------------+
| default_passwd              | admin                                                                       |
+-----------------------------+-----------------------------------------------------------------------------+
| nvram_path                  | /Library/Application Support/Veertu/Anka/uefi/anka_vars.fd                  |
+-----------------------------+-----------------------------------------------------------------------------+
| vm_lib_dir                  | /Users/nathanpierce/Library/Application Support/Veertu/Anka/vm_lib/         |
+-----------------------------+-----------------------------------------------------------------------------+
| anka_image_maker_executable | /Library/Application Support/Veertu/Anka/bin/anka_image                     |
+-----------------------------+-----------------------------------------------------------------------------+
| mac_prefix_file             | /Users/nathanpierce/.anka/mac_prefix.txt                                    |
+-----------------------------+-----------------------------------------------------------------------------+
| process_type                | Interactive                                                                 |
+-----------------------------+-----------------------------------------------------------------------------+
| addons_pkg                  | com.veertu.anka.guestaddons.pkg                                             |
+-----------------------------+-----------------------------------------------------------------------------+
| log_dir                     | /Users/nathanpierce/Library/Logs/Anka/                                      |
+-----------------------------+-----------------------------------------------------------------------------+
| operations_timeout          | 300                                                                         |
+-----------------------------+-----------------------------------------------------------------------------+
| vnc_password                | admin                                                                       |
+-----------------------------+-----------------------------------------------------------------------------+
| nice                        | 0                                                                           |
+-----------------------------+-----------------------------------------------------------------------------+
| product_root                | /Library/Application Support/Veertu/Anka                                    |
+-----------------------------+-----------------------------------------------------------------------------+
| addons_disk_tool            | /Library/Application Support/Veertu/Anka/tools/addons_create_update_disk.sh |
+-----------------------------+-----------------------------------------------------------------------------+
| time_sync                   | 1                                                                           |
+-----------------------------+-----------------------------------------------------------------------------+
| ca_bundle                   | /Users/nathanpierce/.anka/ca-bundle.crt                                     |
+-----------------------------+-----------------------------------------------------------------------------+
| max_vms_allowed             | 255                                                                         |
+-----------------------------+-----------------------------------------------------------------------------+
| portfwd_base                | 10000                                                                       |
+-----------------------------+-----------------------------------------------------------------------------+
| addons_postupdate           | /Library/Application Support/Veertu/Anka/tools/addons_postupdate.sh         |
+-----------------------------+-----------------------------------------------------------------------------+
| bridge_name                 |                                                                             |
+-----------------------------+-----------------------------------------------------------------------------+
| mac_random_bytes            | 2                                                                           |
+-----------------------------+-----------------------------------------------------------------------------+
| anka_executable             | /Library/Application Support/Veertu/Anka/bin/ankahv                         |
+-----------------------------+-----------------------------------------------------------------------------+
| block_nocache               | True                                                                        |
+-----------------------------+-----------------------------------------------------------------------------+
| default_user                | anka                                                                        |
+-----------------------------+-----------------------------------------------------------------------------+
| vm_lock_dir                 | /Users/nathanpierce/Library/Application Support/Veertu/Anka/vm_lib/.locks   |
+-----------------------------+-----------------------------------------------------------------------------+
| state_lib_dir               | /Users/nathanpierce/Library/Application Support/Veertu/Anka/state_lib/      |
+-----------------------------+-----------------------------------------------------------------------------+
| mac_counter                 | /Users/nathanpierce/.anka/mac_counter.txt                                   |
+-----------------------------+-----------------------------------------------------------------------------+
| uhost_executable            | /Library/Application Support/Veertu/Anka/bin/uhost                          |
+-----------------------------+-----------------------------------------------------------------------------+
| addons_identifier           | com.veertu.anka.addons.core.pkg                                             |
+-----------------------------+-----------------------------------------------------------------------------+
| addons_image                | /Library/Application Support/Veertu/Anka/guestaddons/anka-addons-mac.iso    |
+-----------------------------+-----------------------------------------------------------------------------+
| allow_nonunique_names       | False                                                                       |
+-----------------------------+-----------------------------------------------------------------------------+
| permanent_mac_prefix        | 86,69,69                                                                    |
+-----------------------------+-----------------------------------------------------------------------------+
| anka_viewer                 | /Applications/Anka.app                                                      |
+-----------------------------+-----------------------------------------------------------------------------+
| img_lib_dir                 | /Users/nathanpierce/Library/Application Support/Veertu/Anka/img_lib/        |
+-----------------------------+-----------------------------------------------------------------------------+
| puller_threads              | 4                                                                           |
+-----------------------------+-----------------------------------------------------------------------------+
| trim_disk                   | True                                                                        |
+-----------------------------+-----------------------------------------------------------------------------+
| log_file                    | anka.log                                                                    |
+-----------------------------+-----------------------------------------------------------------------------+
| bios_path                   | /Library/Application Support/Veertu/Anka/uefi/anka_code.fd                  |
+-----------------------------+-----------------------------------------------------------------------------+
| adresses_per_byte           | 250                                                                         |
+-----------------------------+-----------------------------------------------------------------------------+
| table_fmt                   | grid                                                                        |
+-----------------------------+-----------------------------------------------------------------------------+
{{</ highlight >}}

> You can see a value for a specific configuration parameter with `anka config $PARAM_NAME`.

- The default VM username is `anka`
- The default VM password is `admin`
- The default storage locations for Anka VM Templates & Tags are:
  - vm_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/vm_lib/`
  - state_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/state_lib/`
  - img_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/img_lib/`
- The log directory is `/Users/XXX/Library/Logs/Anka/` and log file is `anka.log`.

## Modifying the configuration

### Using external storage

If the Nodes on which you installed Anka have insufficient storage for Anka VMs, you can tell Anka to use external storage.

There are three configuration parameters to control location for storing Anka VMs.

Assuming you want to store your Templates on **/Volumes/ExternalDrive/**, perform these steps:

1. `anka config img_lib_dir /Volumes/ExternalDrive/image_lib`
2. `anka config state_lib_dir /Volumes/ExternalDrive/state_lib`
3. `anka config vm_lib_dir /Volumes/ExternalDrive/vm_lib`