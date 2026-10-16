---
title: "Optimizing your VM"
linkTitle: "Optimizing your VM"
weight: 7
description: >
  Post-creation tuning for CI and automation workloads
aliases:
  - "/anka-virtualization-cli/getting-started/optimizing-your-vm/"
---

Apply these changes inside the guest after [creating]({{< relref "anka-virtualization-cli/getting-started/creating-vms/_index.md" >}}) and before you push a template to the registry.

## Disable Spotlight indexing

```bash
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes" || true
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network" || true
sudo killall mds || true
sleep 60
sudo mdutil -a -i off / || true
sudo mdutil -a -i off || true
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist || true
sudo rm -rf /.Spotlight-V100/*
rm -rf ~/Library/Metadata/CoreSpotlight/ || true
killall -KILL Spotlight spotlightd mds || true
sudo rm -rf /System/Volumes/Data/.Spotlight-V100 || true
```

## Disable SIP

`anka create` disables SIP automatically. GUI-created VMs need [manual SIP disable]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-anka-gui.md#disable-sip-recommended-for-ci" >}}).

With SIP enabled, `syspolicyd` scans processes and slows CI workloads.

## Disable unused macOS services

These scripts stop launchd agents and daemons that a CI guest does not use. Examples are Siri, iCloud, Photos, and analytics. In one guest test, RAM use fell by almost 3 GB.

Run the script in the guest after you disable SIP. You can also run the same script on the host when SIP is disabled on the host.

The scripts disable services for user ID 501 (`gui/501`). The default Anka user is user ID 501.

{{< hint warning >}}
Both scripts disable Screen Sharing (`com.apple.screensharing` and the related agents). Anka enables Screen Sharing so you can open the guest with VNC. Delete those names from the list when you still need VNC. Remote Login stays enabled, so SSH continues to work.

The scripts record the disabled services in `/private/var/db/com.apple.xpc.launchd/`. To restore the services, delete those records and reboot:

```bash
sudo rm -r /private/var/db/com.apple.xpc.launchd/*
```

Adapt the lists to your workload. Source: [b0gdanw](https://gist.github.com/b0gdanw), based on [disable.sh by pwnsdx](https://gist.github.com/pwnsdx/d87b034c4c0210b988040ad2f85a68d3).
{{< /hint >}}

### macOS 11 through macOS 15

Use this script on macOS Big Sur (11) through macOS Sequoia (15). Source: [Disable Sequoia bloatware](https://gist.github.com/b0gdanw/b349f5f72097955cf18d6e7d8035c665).

{{< code-collapse lang="zsh" >}}
#!/bin/zsh
# WARNING! The script is meant to show how and what can be disabled. Do not use it as it is. Adapt it to your needs.
# Credit: Original idea and script disable.sh by pwnsdx https://gist.github.com/pwnsdx/d87b034c4c0210b988040ad2f85a68d3
# Disabling unwanted services on macOS Big Sur (11), macOS Monterey (12), macOS Ventura (13), macOS Sonoma (14) and macOS Sequoia (15)
# Disabling SIP is required ("csrutil disable" from Terminal in Recovery)
# Modifications are written in /private/var/db/com.apple.xpc.launchd/ disabled.plist, disabled.501.plist
# To revert, delete /private/var/db/com.apple.xpc.launchd/ disabled.plist and disabled.501.plist and reboot; sudo rm -r /private/var/db/com.apple.xpc.launchd/*

# user
TODISABLE=()

TODISABLE+=('com.apple.accessibility.MotionTrackingAgent' \
'com.apple.accessibility.axassetsd' \
'com.apple.ap.adprivacyd' \
'com.apple.ap.promotedcontentd' \
'com.apple.assistant_service' \
'com.apple.assistantd' \
'com.apple.assistant_cdmd' \
'com.apple.avconferenced' \
'com.apple.BiomeAgent' \
'com.apple.biomesyncd' \
'com.apple.calaccessd' \
'com.apple.CallHistoryPluginHelper' \
'com.apple.chronod' \
'com.apple.cloudd' \
'com.apple.cloudpaird' \
'com.apple.cloudphotod' \
'com.apple.CloudSettingsSyncAgent' \
'com.apple.CommCenter-osx' \
'com.apple.ContextStoreAgent' \
'com.apple.CoreLocationAgent' \
'com.apple.corespeechd' \
'com.apple.dataaccess.dataaccessd' \
'com.apple.duetexpertd' \
'com.apple.familycircled' \
'com.apple.familycontrols.useragent' \
'com.apple.familynotificationd' \
'com.apple.financed' \
'com.apple.findmy.findmylocateagent' \
'com.apple.followupd' \
'com.apple.gamed' \
'com.apple.generativeexperiencesd' \
'com.apple.geoanalyticsd' \
'com.apple.geodMachServiceBridge' \
'com.apple.helpd' \
'com.apple.homed' \
'com.apple.icloud.fmfd' \
'com.apple.iCloudNotificationAgent' \
'com.apple.icloudmailagent' \
'com.apple.iCloudUserNotifications' \
'com.apple.icloud.searchpartyuseragent' \
'com.apple.imagent' \
'com.apple.imautomatichistorydeletionagent' \
'com.apple.imtransferagent' \
'com.apple.inputanalyticsd' \
'com.apple.intelligenceflowd' \
'com.apple.intelligencecontextd' \
'com.apple.intelligenceplatformd' \
'com.apple.itunescloudd' \
'com.apple.knowledge-agent' \
'com.apple.knowledgeconstructiond' \
'com.apple.ManagedClientAgent.enrollagent' \
'com.apple.Maps.pushdaemon' \
'com.apple.Maps.mapssyncd' \
'com.apple.maps.destinationd' \
'com.apple.mediaanalysisd' \
'com.apple.mediastream.mstreamd' \
'com.apple.naturallanguaged' \
'com.apple.navd' \
'com.apple.newsd' \
'com.apple.parsec-fbf' \
'com.apple.parsecd' \
'com.apple.passd' \
'com.apple.photoanalysisd' \
'com.apple.photolibraryd' \
'com.apple.progressd' \
'com.apple.protectedcloudstorage.protectedcloudkeysyncing' \
'com.apple.quicklook' \
'com.apple.quicklook.ui.helper' \
'com.apple.quicklook.ThumbnailsAgent' \
'com.apple.rapportd' \
'com.apple.rapportd-user' \
'com.apple.remindd' \
'com.apple.replicatord' \
'com.apple.routined' \
'com.apple.screensharing.agent' \
'com.apple.screensharing.menuextra' \
'com.apple.screensharing.MessagesAgent' \
'com.apple.ScreenTimeAgent' \
'com.apple.SSInvitationAgent' \
'com.apple.security.cloudkeychainproxy3' \
'com.apple.sharingd' \
'com.apple.sidecar-hid-relay' \
'com.apple.sidecar-relay' \
'com.apple.siriactionsd' \
'com.apple.Siri.agent' \
'com.apple.siriinferenced' \
'com.apple.sirittsd' \
'com.apple.SiriTTSTrainingAgent' \
'com.apple.macos.studentd' \
'com.apple.siriknowledged' \
'com.apple.suggestd' \
'com.apple.tipsd' \
'com.apple.telephonyutilities.callservicesd' \
'com.apple.TMHelperAgent' \
'com.apple.triald' \
'com.apple.universalaccessd' \
'com.apple.UsageTrackingAgent' \
'com.apple.videosubscriptionsd' \
'com.apple.voicebankingd' \
'com.apple.watchlistd' \
'com.apple.weatherd')

for agent in "${TODISABLE[@]}"
do
 launchctl bootout gui/501/${agent}
 launchctl disable gui/501/${agent}
done

# system
TODISABLE=()

TODISABLE+=('com.apple.analyticsd' \
'com.apple.audioanalyticsd' \
'com.apple.backupd' \
'com.apple.backupd-helper' \
'com.apple.biomed' \
'com.apple.cloudd' \
'com.apple.coreduetd' \
'com.apple.dhcp6d' \
'com.apple.ecosystemanalyticsd' \
'com.apple.familycontrols' \
'com.apple.findmymac' \
'com.apple.findmymacmessenger' \
'com.apple.findmy.findmybeaconingd' \
'com.apple.ftp-proxy' \
'com.apple.GameController.gamecontrollerd' \
'com.apple.icloud.findmydeviced' \
'com.apple.icloud.searchpartyd' \
'com.apple.locationd' \
'com.apple.ManagedClient.cloudconfigurationd' \
'com.apple.modelmanagerd' \
'com.apple.netbiosd' \
'com.apple.rapportd' \
'com.apple.screensharing' \
'com.apple.triald.system' \
'com.apple.wifianalyticsd')

for daemon in "${TODISABLE[@]}"
do
 sudo launchctl bootout system/${daemon}
 sudo launchctl disable system/${daemon}
done

sudo launchctl bootout user/205/com.apple.geod
sudo launchctl disable user/205/com.apple.geod
{{< /code-collapse >}}

### macOS 27

Use this script on macOS Golden Gate (27). Source: [Disable macOS Golden Gate bloatware](https://gist.github.com/b0gdanw/47fe6a8c1dfec0a9b11edded58565dd5).

{{< code-collapse lang="zsh" >}}
#!/bin/zsh
# WARNING! The script is meant to show how and what can be disabled. Do not use it as it is. Adapt it to your needs.
# Credit: Original idea and script disable.sh by pwnsdx https://gist.github.com/pwnsdx/d87b034c4c0210b988040ad2f85a68d3
# Disabling unwanted services on macOS Golden Gate (27)
# Disabling SIP is required ("csrutil disable" from Terminal in Recovery)
# Modifications are written in /private/var/db/com.apple.xpc.launchd/ disabled.plist, disabled.501.plist
# To revert, delete /private/var/db/com.apple.xpc.launchd/ disabled.plist and disabled.501.plist and reboot. From Terminal: sudo rm -r /private/var/db/com.apple.xpc.launchd/*

# user
TODISABLE=()

TODISABLE+=('com.apple.accessibility.MotionTrackingAgent' \
'com.apple.accessibility.axassetsd' \
'com.apple.ap.adprivacyd' \
'com.apple.ap.promotedcontentd' \
'com.apple.assistant_service' \
'com.apple.assistantd' \
'com.apple.assistant_cdmd' \
'com.apple.avconferenced' \
'com.apple.BiomeAgent' \
'com.apple.biomesyncd' \
'com.apple.calaccessd' \
'com.apple.CallHistoryPluginHelper' \
'com.apple.campo' \
'com.apple.chronod' \
'com.apple.cloudd' \
'com.apple.cloudpaird' \
'com.apple.cloudphotod' \
'com.apple.CloudSettingsSyncAgent' \
'com.apple.CommCenter-osx' \
'com.apple.ContextStoreAgent' \
'com.apple.CoreLocationAgent' \
'com.apple.corespeechd' \
'com.apple.dataaccess.dataaccessd' \
'com.apple.duetexpertd' \
'com.apple.familycircled' \
'com.apple.familycontrols.useragent' \
'com.apple.familynotificationd' \
'com.apple.financed' \
'com.apple.findmy.findmylocateagent' \
'com.apple.followupd' \
'com.apple.gamed' \
'com.apple.generativeexperiencesd' \
'com.apple.geoanalyticsd' \
'com.apple.geod' \
'com.apple.geodMachServiceBridge' \
'com.apple.helpd' \
'com.apple.homed' \
'com.apple.iCloudNotificationAgent' \
'com.apple.icloudmailagent' \
'com.apple.iCloudUserNotifications' \
'com.apple.icloud.searchpartyuseragent' \
'com.apple.imagent' \
'com.apple.imautomatichistorydeletionagent' \
'com.apple.imtransferagent' \
'com.apple.inputanalyticsd' \
'com.apple.intelligenceflowd' \
'com.apple.intelligencecontextd' \
'com.apple.intelligenceplatformd' \
'com.apple.intelligentroutingd' \
'com.apple.itunescloudd' \
'com.apple.knowledge-agent' \
'com.apple.knowledgeconstructiond' \
'com.apple.ManagedClientAgent.enrollagent' \
'com.apple.Maps.pushdaemon' \
'com.apple.Maps.mapssyncd' \
'com.apple.maps.destinationd' \
'com.apple.mediaanalysisd' \
'com.apple.mediastream.mstreamd' \
'com.apple.ModelCatalogAgent' \
'com.apple.naturallanguaged' \
'com.apple.navd' \
'com.apple.newsd' \
'com.apple.parsec-fbf' \
'com.apple.parsecd' \
'com.apple.passd' \
'com.apple.photoanalysisd' \
'com.apple.photolibraryd' \
'com.apple.progressd' \
'com.apple.protectedcloudstorage.protectedcloudkeysyncing' \
'com.apple.quicklook' \
'com.apple.quicklook.ui.helper' \
'com.apple.quicklook.ThumbnailsAgent' \
'com.apple.rapportd-user' \
'com.apple.remindd' \
'com.apple.replicatord' \
'com.apple.routined' \
'com.apple.screensharing.agent' \
'com.apple.screensharing.menuextra' \
'com.apple.screensharing.MessagesAgent' \
'com.apple.ScreenTimeAgent' \
'com.apple.SSInvitationAgent' \
'com.apple.security.cloudkeychainproxy3' \
'com.apple.sharingd' \
'com.apple.sidecar-hid-relay' \
'com.apple.sidecar-relay' \
'com.apple.siriactionsd' \
'com.apple.Siri.agent' \
'com.apple.siriinferenced' \
'com.apple.sirittsd' \
'com.apple.SiriTTSTrainingAgent' \
'com.apple.macos.studentd' \
'com.apple.siriknowledged' \
'com.apple.spotlightknowledged.importer' \
'com.apple.spotlightknowledged.updater' \
'com.apple.suggestd' \
'com.apple.tipsd' \
'com.apple.telephonyutilities.callservicesd' \
'com.apple.TMHelperAgent' \
'com.apple.triald' \
'com.apple.universalaccessd' \
'com.apple.UsageTrackingAgent' \
'com.apple.videosubscriptionsd' \
'com.apple.visualintelligenced' \
'com.apple.voicebankingd' \
'com.apple.watchlistd' \
'com.apple.weatherd')

for agent in "${TODISABLE[@]}"
do
 launchctl bootout gui/501/${agent}
 launchctl disable gui/501/${agent}
done

# system
TODISABLE=()

TODISABLE+=('com.apple.analyticsd' \
'com.apple.audioanalyticsd' \
'com.apple.backupd' \
'com.apple.backupd-helper' \
'com.apple.biomed' \
'com.apple.cloudd' \
'com.apple.coreduetd' \
'com.apple.cloudtelemetryd' \
'com.apple.dhcp6d' \
'com.apple.ecosystemanalyticsd' \
'com.apple.familycontrols' \
'com.apple.findmymac' \
'com.apple.findmymacmessenger' \
'com.apple.findmy.findmybeaconingd' \
'com.apple.ftp-proxy' \
'com.apple.GameController.gamecontrollerd' \
'com.apple.icloud.searchpartyd' \
'com.apple.locationd' \
'com.apple.modelmanagerd' \
'com.apple.modelcatalogd' \
'com.apple.netbiosd' \
'com.apple.rapportd' \
'com.apple.screensharing' \
'com.apple.triald.system' \
'com.apple.wifianalyticsd')

for daemon in "${TODISABLE[@]}"
do
 sudo launchctl bootout system/${daemon}
 sudo launchctl disable system/${daemon}
done
{{< /code-collapse >}}

## What's next

- [Modifying your VM]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}}) — CPU, RAM, disk, networking
- [VM clones and tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms/vm-clones-and-tags.md" >}})
