


---
date: 2019-12-12T00:00:00-00:00
title: "Install"
linkTitle: "Install"
weight: 1
description: >
  Steps to install Anka Command Line Tool.
---

## Download latest Anka PKG
Download the file from [Anka Build Download page](https://veertu.com/download-anka-build/).

## Install Anka PKG
### Using the ui
Run the `anka.pkg` installer you downloaded. 
### Using the command line
cd to the directory containing the file, then run:
```shell
sudo installer -pkg Anka-xx.x.pkg -target
```

## Installing Anka with nested virtualization
Install Anka in this manner in order to be able to create VMs with nested feature enabled
### Using the ui
Run the `anka.pkg` installer you downloaded, select nested virtualization checkbox in Customizations while installing Anka through the GUI method.  
### Using the command line
cd to the directory containing the file, then run: 
```shell
sudo installer -applyChoiceChangesXML nanka.xml -pkg Ankaxx.pkg -target /
```

## Anka package components
The Anka package includes the Core hypervisor, `anka` guest Addons and Controller Anka agent.

## Verify installation
After running the installer package, `anka` is found at `/usr/local/bin/anka`.
Verify the installation by running `anka version` command.


## License activation
Activate your license using the anka license command and your license key:
```
sudo anka license activate <key>
```


