---
title: "VM time and date (NTP) incorrect"
weight: 1
---

## Scenario

Post start, you find your VM's time and date is not up to date. Sometimes off by weeks or months.

## Common Causes

1. Rarely the screensaver inside of the VM turning on can disable time sync.
1. The VM can go to sleep if it's long-running and cause NTP to get out of sync.
1. This happens when the `sudo sntp -sS time.apple.com` fails inside of the VM post-start. We've disabled automatic NTP syncing to support suspended VM states (it causes problems), so our addons will execute the `sntp` command post-start.
   - Addons log to `/var/log/anka.log` inside of the VM and may indicate/log what went wrong.

## Solution

1. Set screensaver to Never.
1. Enable "Prevent automatic sleeping when the display is off" under Energy settings.
1. You can manually run `sudo sntp -sS time.apple.com` in your jobs to ensure macOS is synced properly.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
