---
date: 2020-01-27
title: "Current Changelog"
linkTitle: "Current Changelog"
weight: 11
description: >
  Published March 23, 2020 
---

## Changelog

### Change Log Anka Controller & Registry combined package (Mac) version 1.7.0 - Mar 23, 2020
- Bug Fix : ankacluster doesn't pass max vm count to agent
- Bug Fix : Node upgrade request is sometimes sent more than once
- Bug Fix : Distribute not showing progress
- Bug Fix : Controller moves from ‘enterprise’ to ‘basic’ after uninstall and upgrade anka from 2.2.1 to 2.2.2
- New feature : Display node storage usage information in the controller dashboard
- New feature : Show size of templates in the controller dashboard
- New feature : Show a link to the jenkins job that the vm is running in the controller dashboard
- New feature : Put prompt while deleting template from the controller dashboard
- New feature : add reserve disk flag in ankacluster command
- New feature : Add the option to recover from a crash while uploading files to registry

### Change Log Anka Controller & Registry combined package (Linux) version 1.7.0 - Mar 23, 2020
- Bug Fix : ankacluster doesn't pass max vm count to agent
- Bug Fix : Node upgrade request is sometimes sent more than once
- Bug Fix : Distribute not showing progress
- Bug Fix : Controller moves from ‘enterprise’ to ‘basic’ after uninstall and upgrade anka from 2.2.1 to 2.2.2
- New feature : Display node storage usage information in the controller dashboard
- New feature : Show size of templates in the controller dashboard
- New feature : Show a link to the jenkins job that the vm is running in the controller dashboard
- New feature : Put prompt while deleting template from the controller dashboard
- New feature : add reserve disk flag in ankacluster command
- New feature : Add the option to recover from a crash while uploading files to registry

### Change Log Jenkins Plugin version 1.23.0 - Mar 23, 2020
- Bug Fix : Controller IP is inaccessible from the Jenkins and Jenkins page is stuck on “loading” forever

***Note*** Currently issues in amazon-ecs-plugin version 1.26 cause problems with Jenkins-Anka Plugin. In order to fix this, downgrade to amazon-ecs-plugin version 1.22 or disable amazon-ecs-plugin. Check https://github.com/jenkinsci/amazon-ecs-plugin/issues/158 for more details.

### Change Log Anka 2.2.2 change - Mar 03, 2020
- Bug Fix : Nested virtualization not working since rel 2.2.0
- Bug Fix : Anka command failed: time data ‘2020-02-27T03:52:23Z’ does not match format ‘%d %b %Y %H:%M:%S’
- Bug Fix : license pass-through from root VM to nested VM doesn't work
- Bug Fix : anka run sources both .bash_profile and .profile. Requires`anka start -u` for existing VM templates.
- Bug Fix : after rebooting a running vm with anka run VM sudo reboot`,  network doesn't work.
- New feature : Allow to specify display physical(DPI) parameters. Requires `anka start -u` for existing VM templates.

***NOTE*** There is no need to upgrade the VM templates from previous anka version 2.2.1 to version 2.2.2, unless you need to use the items from the above list which explicitly state the requirement of `anka start -u`.
