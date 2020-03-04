---
date: 2020-01-27
title: "Current Changelog"
linkTitle: "Current Changelog"
weight: 11
description: >
  Published March 03, 2020 
---

## Changelog

### Change Log Anka 2.2.2 change - Mar 03, 2020
- Bug Fix : Nested virtualization not working since rel 2.2.0
- Bug Fix : Anka command failed: time data ‘2020-02-27T03:52:23Z’ does not match format ‘%d %b %Y %H:%M:%S’
- Bug Fix : license pass-through from root VM to nested VM doesn't work
- Bug Fix : anka run sources both .bash_profile and .profile. Requires`anka start -u` for existing VM templates.
- Bug Fix : after rebooting a running vm with anka run VM sudo reboot`,  network doesn't work.
- New feature : Allow to specify display physical(DPI) parameters. Requires `anka start -u` for existing VM templates.

***NOTE*** There is no need to upgrade the VM templates from previous anka version 2.2.1 to version 2.2.2, unless you need to use the items from the above list which explicitly state the requirement of `anka start -u`.
