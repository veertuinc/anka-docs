---
title: "VM does not have Bonjour"
linkTitle: "VM does not have Bonjour"
weight: 1
---

## Scenario

Anka VM doesn't seem to have Bonjour available inside so that iOS simulator libraries can use it.

## Common Causes

1. Bonjour is disabled by us for security reasons

## Solution

```bash
sudo launchctl unload /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist
sudo defaults write /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements -bool No
sudo launchctl load /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist
```

Once Bonjour is enabled inside of the VM, and if you're still concerned about security, you can set `anka modify {template name} set network-card --no-local` to prevent VM networking from being exposed on the host.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

