---
date: 2019-12-12T00:00:00-00:00
title: "Command Reference"
linkTitle: "Command Reference"
weight: 8
description: >
  Anka CLI Command Reference 
---

{{< include file="./shared/content/en/docs/Anka CLI/partials/_index.md" >}}

## Attach
{{< include file="./shared/content/en/docs/Anka CLI/partials/attach/_index.md" >}}
## Clone
{{< include file="./shared/content/en/docs/Anka CLI/partials/clone/_index.md" >}}
## Config
{{< include file="./shared/content/en/docs/Anka CLI/partials/config/_index.md" >}}
## Create
{{< include file="./shared/content/en/docs/Anka CLI/partials/create/_index.md" >}}

> [Advanced usage guide for `anka create`](({{< relref "docs/Anka CLI/creating-templates-and-tags.md" >}}))

## Delete
{{< include file="./shared/content/en/docs/Anka CLI/partials/delete/_index.md" >}}
## Describe
{{< include file="./shared/content/en/docs/Anka CLI/partials/describe/_index.md" >}}
## Detach
{{< include file="./shared/content/en/docs/Anka CLI/partials/detach/_index.md" >}}
## License
{{< include file="./shared/content/en/docs/Anka CLI/partials/license/_index.md" >}}
### license accept-eula
{{< include file="./shared/content/en/docs/Anka CLI/partials/license/accept-eula/_index.md" >}}
### license activate
{{< include file="./shared/content/en/docs/Anka CLI/partials/license/activate/_index.md" >}}
### license remove
{{< include file="./shared/content/en/docs/Anka CLI/partials/license/remove/_index.md" >}}
### license show
{{< include file="./shared/content/en/docs/Anka CLI/partials/license/show/_index.md" >}}

> To see current core consumption information for your license, use `anka license show <licensekey>`

### license validate
{{< include file="./shared/content/en/docs/Anka CLI/partials/license/validate/_index.md" >}}
## List
{{< include file="./shared/content/en/docs/Anka CLI/partials/list/_index.md" >}}
## Modify
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/_index.md" >}}
### modify {template} add
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/add/_index.md" >}}
#### modify {template} add hard-drive
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/add/hard-drive/_index.md" >}}
#### modify {template} add network-card
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/add/network-card/_index.md" >}}
#### modify {template} add optical-drive
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/add/optical-drive/_index.md" >}}
#### modify {template} add port-forwarding
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/add/port-forwarding/_index.md" >}}

##### **EXAMPLE** - add port-forwarding
```shell
❯ sudo anka modify 10.15.4 add port-forwarding --guest-port 22 ssh

❯ sudo anka describe 10.15.4

. . .

port_forwarding_rules

+------------+--------------+-------------+------------+-----------+-------------+
|   net_card |   guest_port | rule_name   | protocol   |   host_ip |   host_port |
+============+==============+=============+============+===========+=============+
|          0 |           22 | ssh         | tcp        |         0 |           0 |
+------------+--------------+-------------+------------+-----------+-------------+

❯ sudo anka start 10.15.4
. . . 
port_forwarding

+--------------+-------------+------------+--------+-----------+
|   guest_port |   host_port | protocol   | name   | host_ip   |
+==============+=============+============+========+===========+
|           22 |       10000 | tcp        | ssh    | 0.0.0.0   |
+--------------+-------------+------------+--------+-----------+

❯ ssh anka@localhost -p 10000
Password:
Last login: Mon Apr  6 12:45:50 2020
Mac-mini:~ anka
```

> Unless you customize the host_port, we default to using 10000, 10001, and so on for each VM that's started. Ensure that both the Controller server and any CI servers (where you host Jenkins for example) can reach the Node's ports in your firewall rules.

#### modify {template} add usb-device
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/add/usb-device/_index.md" >}}
### modify {template} delete
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/_index.md" >}}
#### modify {template} delete custom-variable
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/custom-variable/_index.md" >}}
#### modify {template} delete hard-drive
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/hard-drive/_index.md" >}}
#### modify {template} delete network-card
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/network-card/_index.md" >}}
#### modify {template} delete optical-drive
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/optical-drive/_index.md" >}}
#### modify {template} delete policy
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/policy/_index.md" >}}
#### modify {template} delete port-forwarding
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/port-forwarding/_index.md" >}}
#### modify {template} delete usb-device
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/delete/usb-device/_index.md" >}}
### modify {template} set
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/_index.md" >}}
#### modify {template} set cpu
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/cpu/_index.md" >}}
#### modify {template} set custom-variable
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/custom-variable/_index.md" >}}

##### EXAMPLE - set custom-variable

```shell
sudo anka modify {template} set custom-variable hw.UUID "GUID"
sudo anka modify {template} set custom-variable hw.serial 'MySerial'
```

You can set the following custom variables:

* boot-args                   - NVRAM boot arguments
* hw.uuid                     - Hardware UUID
* hw.serial                   - Serial Number (system)
* hw.manufacturer             - SMBIOS parameter (Reserved)
* hw.product                  - SMBIOS parameter (Reserved)
* hw.family                   - SMBIOS parameter (Reserved)
* hw.board                    - SMBIOS parameter (Reserved)

#### modify {template} set description
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/description/_index.md" >}}
#### modify {template} set display
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/display/_index.md" >}}
#### modify {template} set hard-drive
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/hard-drive/_index.md" >}}
#### modify {template} set name
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/name/_index.md" >}}
#### modify {template} set nested
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/nested/_index.md" >}}
#### modify {template} set network-card
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/network-card/_index.md" >}}
#### modify {template} set policy
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/policy/_index.md" >}}
#### modify {template} set ram
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/set/ram/_index.md" >}}
### modify {template} show
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/show/_index.md" >}}
#### modify {template} show custom-variables
{{< include file="./shared/content/en/docs/Anka CLI/partials/modify/show/custom-variables/_index.md" >}}
## Mount
{{< include file="./shared/content/en/docs/Anka CLI/partials/mount/_index.md" >}}
## Reboot
{{< include file="./shared/content/en/docs/Anka CLI/partials/reboot/_index.md" >}}
## Registry
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/_index.md" >}}
### registry add
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/add/_index.md" >}}
### registry check-download-size
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/check-download-size/_index.md" >}}
### registry delete
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/delete/_index.md" >}}
### registry describe
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/describe/_index.md" >}}
### registry list
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/list/_index.md" >}}
### registry list-repos
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/list-repos/_index.md" >}}
### registry pull
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/pull/_index.md" >}}
### registry push
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/push/_index.md" >}}
### registry set
{{< include file="./shared/content/en/docs/Anka CLI/partials/registry/set/_index.md" >}}
## Run
{{< include file="./shared/content/en/docs/Anka CLI/partials/run/_index.md" >}}
## Show
{{< include file="./shared/content/en/docs/Anka CLI/partials/show/_index.md" >}}
## Start
{{< include file="./shared/content/en/docs/Anka CLI/partials/start/_index.md" >}}
## Stop
{{< include file="./shared/content/en/docs/Anka CLI/partials/stop/_index.md" >}}
## Suspend
{{< include file="./shared/content/en/docs/Anka CLI/partials/suspend/_index.md" >}}
## Unmount
{{< include file="./shared/content/en/docs/Anka CLI/partials/unmount/_index.md" >}}
## Usb
{{< include file="./shared/content/en/docs/Anka CLI/partials/usb/_index.md" >}}
### usb claim
{{< include file="./shared/content/en/docs/Anka CLI/partials/usb/claim/_index.md" >}}

##### EXAMPLE - claiming with Yubikey 

Unloading of ifdreader service should precede claiming to avoid access collisions:

```shell
sudo launchctl stop com.apple.ifdreader
sudo anka usb claim $LOCATION
```

### usb list
{{< include file="./shared/content/en/docs/Anka CLI/partials/usb/list/_index.md" >}}
### usb release
{{< include file="./shared/content/en/docs/Anka CLI/partials/usb/release/_index.md" >}}
## Version
{{< include file="./shared/content/en/docs/Anka CLI/partials/version/_index.md" >}}
## View
{{< include file="./shared/content/en/docs/Anka CLI/partials/view/_index.md" >}}