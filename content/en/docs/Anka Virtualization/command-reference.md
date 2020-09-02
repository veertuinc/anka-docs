---
date: 2019-12-12T00:00:00-00:00
title: "Command Reference"
linkTitle: "Command Reference"
weight: 8
description: >
  Anka CLI Command Reference 
---

{{< include file="./shared/content/en/docs/Anka Virtualization/partials/_index.md" >}}

## Attach
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/attach/_index.md" >}}
## Clone
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/clone/_index.md" >}}
## Config
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/config/_index.md" >}}
## Create
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/create/_index.md" >}}

> [Advanced usage guide for `anka create`](({{< relref "docs/Getting Started/creating-your-first-vm.md" >}}))

## Delete
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/delete/_index.md" >}}
## Describe
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/describe/_index.md" >}}
## Detach
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/detach/_index.md" >}}
## License
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/license/_index.md" >}}
### license accept-eula
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/license/accept-eula/_index.md" >}}
### license activate
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/license/activate/_index.md" >}}
### license remove
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/license/remove/_index.md" >}}
### license show
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/license/show/_index.md" >}}

> To see current core consumption information for your license, use `anka license show <licensekey>`

### license validate
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/license/validate/_index.md" >}}
## List
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/list/_index.md" >}}
## Modify
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/_index.md" >}}
### modify {templateName} add
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/add/_index.md" >}}
#### modify {templateName} add hard-drive
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/add/hard-drive/_index.md" >}}
#### modify {templateName} add network-card
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/add/network-card/_index.md" >}}
#### modify {templateName} add optical-drive
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/add/optical-drive/_index.md" >}}
#### modify {templateName} add port-forwarding
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/add/port-forwarding/_index.md" >}}
{{< include file="shared/content/en/docs/Anka Virtualization/partials/modify/add/port-forwarding/_example.md" >}}


#### modify {templateName} add usb-device
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/add/usb-device/_index.md" >}}
### modify {templateName} delete
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/_index.md" >}}
#### modify {templateName} delete custom-variable
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/custom-variable/_index.md" >}}
#### modify {templateName} delete hard-drive
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/hard-drive/_index.md" >}}
#### modify {templateName} delete network-card
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/network-card/_index.md" >}}
#### modify {templateName} delete optical-drive
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/optical-drive/_index.md" >}}
#### modify {templateName} delete policy
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/policy/_index.md" >}}
#### modify {templateName} delete port-forwarding
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/port-forwarding/_index.md" >}}
#### modify {templateName} delete usb-device
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/delete/usb-device/_index.md" >}}
### modify {templateName} set
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/_index.md" >}}
#### modify {templateName} set cpu
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/cpu/_index.md" >}}
#### modify {templateName} set custom-variable
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/custom-variable/_index.md" >}}
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/custom-variable/_example.md" >}}
#### modify {templateName} set description
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/description/_index.md" >}}
#### modify {templateName} set display
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/display/_index.md" >}}
#### modify {templateName} set hard-drive
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/hard-drive/_index.md" >}}
> It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM Template with a smaller amount of available disk and then increase in subsequent Tags.
#### modify {templateName} set name
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/name/_index.md" >}}
#### modify {templateName} set nested
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/nested/_index.md" >}}
#### modify {templateName} set network-card
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/network-card/_index.md" >}}
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/network-card/_example.md" >}}
#### modify {templateName} set policy
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/policy/_index.md" >}}
#### modify {templateName} set ram
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/set/ram/_index.md" >}}
### modify {templateName} show
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/show/_index.md" >}}
#### modify {templateName} show custom-variables
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/modify/show/custom-variables/_index.md" >}}
## Mount
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/mount/_index.md" >}}
## Reboot
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/reboot/_index.md" >}}
## Registry
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/_index.md" >}}
### registry add
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/add/_index.md" >}}
### registry check-download-size
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/check-download-size/_index.md" >}}
### registry delete
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/delete/_index.md" >}}
### registry describe
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/describe/_index.md" >}}
### registry list
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/list/_index.md" >}}
### registry list-repos
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/list-repos/_index.md" >}}
### registry pull
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/pull/_index.md" >}}
### registry push
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/push/_index.md" >}}
### registry set
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/set/_index.md" >}}
## Run
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/run/_index.md" >}}
## Show
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/show/_index.md" >}}
## Start
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/start/_index.md" >}}
## Stop
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/stop/_index.md" >}}
## Suspend
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/suspend/_index.md" >}}
## Unmount
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/unmount/_index.md" >}}
## Usb
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/usb/_index.md" >}}
### usb claim
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/usb/claim/_index.md" >}}
### usb list
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/usb/list/_index.md" >}}
### usb release
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/usb/release/_index.md" >}}
## Version
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/version/_index.md" >}}
## View
{{< include file="./shared/content/en/docs/Anka Virtualization/partials/view/_index.md" >}}