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

## What's next

- [Modifying your VM]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}}) — CPU, RAM, disk, networking
- [VM clones and tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms/vm-clones-and-tags.md" >}})
