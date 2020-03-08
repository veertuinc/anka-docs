---
date: 2020-01-27
title: "Current Changelog"
linkTitle: "Current Changelog"
weight: 11
description: >
  Published March 08, 2020 
---

## Changelog

### Change Log Anka Controller & Registry combined package (Mac) - Mar 08, 2020
- Bug Fix : Jenkins plugin leaves zombie VMs after upgrade to version 2.204
- Bug Fix : Anka registry pull hangs forever when run from agent if VM has a lot of image files
- New feature : Handle vm start failures more robustly
- New feature : Added a fail timeout for template distribution

### Change Log Anka Controller & Registry combined package (Linux) - Mar 08, 2020
- Bug Fix : Jenkins plugin leaves zombie VMs after upgrade to version 2.204
- Bug Fix : Anka registry pull hangs forever when run from agent if VM has a lot of image files
- New feature : Handle vm start failures more robustly
- New feature : Added a fail timeout for template distribution

### Change Log Jenkins Plugin version - Mar 08, 2020
- Bug Fix : Jenkins plugin leaves zombie VMs after Jenkins upgrade to 2.204
- Bug Fix : Jenkins leaves zombie VMs when restarting
- Bug Fix : Jenkins leaves zombie VMs when job has error on JNLP node
- Bug Fix : Jenkins Job changing Jenkins Master node labels
- Bug Fix : Jenkins JNLP Job starts more instances than it should
- Bug Fix : Jenkins config hangs of controller IP is inaccessible
***Note*** Currently issues in amazon-ecs-plugin version 1.26 cause problems with Jenkins-Anka Plugin. In order to fix this, downgrade to amazon-ecs-plugin version 1.22 or disable amazon-ecs-plugin. Check https://github.com/jenkinsci/amazon-ecs-plugin/issues/158 for more details.

### Change Log Anka 2.2.2 change - Mar 03, 2020
- Bug Fix : Nested virtualization not working since rel 2.2.0
- Bug Fix : Anka command failed: time data ‘2020-02-27T03:52:23Z’ does not match format ‘%d %b %Y %H:%M:%S’
- Bug Fix : license pass-through from root VM to nested VM doesn't work
- Bug Fix : anka run sources both .bash_profile and .profile. Requires`anka start -u` for existing VM templates.
- Bug Fix : after rebooting a running vm with anka run VM sudo reboot`,  network doesn't work.
- New feature : Allow to specify display physical(DPI) parameters. Requires `anka start -u` for existing VM templates.

***NOTE*** There is no need to upgrade the VM templates from previous anka version 2.2.1 to version 2.2.2, unless you need to use the items from the above list which explicitly state the requirement of `anka start -u`.
