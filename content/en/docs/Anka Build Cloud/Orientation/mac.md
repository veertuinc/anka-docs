


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

<<<<<<< HEAD
=======
### Logs

###Anka logs

 dir: `/var/log/veertu` 
   `anka.log`- Activity with anka command(Anka Cli commands, Anka Run) is logged here, also this log is not being removed                    but round robined.
   `UUID.log`- UUID is the VM's uuid - it's VM log, usually we check these logs when VM exits abnormally or fails to start. 
               the file is created upon starting a vm. and deleted upon deletion of VM.
   `lopd.log`- is from license auto-upgrade service, it's rarely used and most likely not related to VMs runtime
   `ankanetd.log`- is our new network service, could be analysed if some error related to network are reported


>>>>>>> 2a416ce9273ffa6fbecfeefc16552928d99a243e
### Data
Data will be saved at:  
`/Library/Application Support/Veertu/Anka/registry`















