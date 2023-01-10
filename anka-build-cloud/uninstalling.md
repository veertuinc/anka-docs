---
date: 2019-12-12T00:00:00-00:00
title: "Uninstalling"
linkTitle: "Uninstalling"
weight: 13
description: >
  Steps for uninstalling the Anka Build Cloud package
---

## Mac

### Controller & Registry Combined

From the command line, execute the following command: `sudo /Library/Application\ Support/Veertu/Anka/tools/controller/uninstall.sh`

### Registry Standalone

This requires manually performing the following steps:

```shell
sudo launchctl unload -w /Library/LaunchDaemons/com.veertu.anka.registry.plist
IFS=$'\n'; for pkgfile in $(sudo pkgutil --only-files --files com.veertu.anka.registry.pkg); do sudo rm "/$pkgfile"; done;
sudo pkgutil --forget com.veertu.anka.registry.pkg
```

## Linux / Docker

`docker-compose down` should stop the containers.
