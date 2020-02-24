---
date: 2020-01-27
title: "Current Changelog"
linkTitle: "Current Changelog"
weight: 11
description: >
  Published February 24, 2020 
---

## Changelog

### Change Log Anka 2.2.1 change - Feb 24, 2020
- Bug Fix : RFB server crash 0x0000000104fa0b8c rfb_thr + 1364
- Bug Fix : Crash on suspend
- Bug Fix : ankactl doesn't report vnc_password string
- Bug Fix : Anka VM fails to start under jenkins master account
- Bug Fix : Anka run --wait-network doesn't seem to work
- Bug Fix : set resolution from anka view doesn't work.
- Bug Fix : Anka version takes a few minutes to respond
- Bug Fix : ankactl experiences EMFILE if there are many snapshots/vms in the library
- Bug Fix : Adding MAC address using anka modify for bridge cards result in corrupted config file for VM
- Bug Fix : Collision of anka and guest addons files
- Bug Fix : VNC not working for when resolution of VM is changed to 1000*600
- Bug Fix : Problems to detect IP address
- Bug Fix : VM doesn't get unique IP address if static MAC address specified
- New feature : Support IPv6 in anka rfb server
- New feature : Allow to assign MAC address to VM
- New feature : Create directories if missing with anka run --workdir
- New feature : Send HID events programmatically
- New feature : Create RFB threads on demand only
- New feature : Give proper message in case of a VM trying to access out of range memory

