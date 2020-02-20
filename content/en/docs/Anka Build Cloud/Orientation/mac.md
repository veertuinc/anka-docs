


---
title: "Mac"
linkTitle: "Mac"
weight: 2
description: >
  Where to find installed files and directories on MacOS.
---

## Anka Controller and Registry
### Default Ports
*Anka Controller:* 80  
*Anka Registry:* 8089  

### Binaries and scripts
Anka Controller has only one combined binary. It can run Anka Controller, Anka Registry and ETCD Server.  
Installed at:    
`/Library/Application Support/Veertu/Anka/bin/anka-controller`  
Start script:  
`/usr/local/bin/anka-controllerd`  
Start/Stop script:    
`/usr/local/bin/anka-controller`  
Launcd daemon:  
`/Library/LaunchDaemons/com.veertu.anka.controller.plist`

### Configuration files
Configuration for this package is done by altering the start script at `/usr/local/bin/anka-controllerd`.

### Logs
`/Library/Logs/Veertu/AnkaController`
### Data
ETCD data will be saved at:  
`/Library/Application Support/Veertu/Anka/anka-controller`  
Registry data will be saved at:  
`/Library/Application Support/Veertu/Anka/registry`



## Anka Registry
### Default Ports
80
### Binaries and scripts
Anka Registry for Mac binary is installed at:   
`/Library/Application Support/Veertu/Anka/bin/ankaregd`  
Launchd daemon:  
`/Library/LaunchDaemons/com.veertu.anka.registry.plist`

### Configuration files
Configuration for this package is done by altering the Launcd daemon xml file at `/Library/LaunchDaemons/com.veertu.anka.registry.plist`.




### Data
Data will be saved at:  
`/Library/Application Support/Veertu/Anka/registry`















