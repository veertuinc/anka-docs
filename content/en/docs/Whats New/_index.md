---
date: 2020-01-27
title: "What's New"
linkTitle: "What's New"
weight: 12
description: >
  Published March 23, 2020 
---

## What's New in Anka Version 2.2.3
### Anka 2.2.3 - May 03, 2020

**When using Anka Viewer, provided a way to get out of full screen, after in full screen mode (using the green full screen button on the window's top bar)**

**Updated license terms**

## What's New in Controller Version 1.7.0

**Disk Usage for Node is displayed in the controller Dahsboard and REST API**

![Node Disk Usage Information](/images/whatsnew/Nodestorageinfo.png)

**Size information for Templates and tags is displayed in the controller dashboard and REST API**

![Template Size Information](/images/whatsnew/templatesize.png)

**Jenkins job/Node information is displayed for the VM provisioned in the controller dashboard**

![Jenkins Job Information](/images/whatsnew/JenkinsInfoportal.png)

**New reserve disk flag in ankacluster join command**

When `–reserve-space` flag is set, controller will always reserve the disk space before pulling VM template on the node. If there is not enough disk space after allocating for `–reserve-space`, then controller will not pull the Vm Template. This flag is provided to avoid scenario where there is no disk psace left on the node to accomodate for extra disk usage duing builds inside the VM.

![reservedisk ankacluster flah](/images/whatsnew/ankaclusterreservediskspace.png)

## What's New in Anka Version 2.2.2

**License passthrough from host to nested enabled Anka VM**

This feature enables you to install Anka binary package inside an Anka macOS VM and use it create and run Anka VMs.

1. Enable nested for the Anka VM in which you want to install Anka binary.
2. Install Anka binary package inside the VM.
2. Execute `anka config propagate_license 1` on the host machine.
3. Stop and start the nested enabled VM.
4. Check with `sudo anka license show` inside the VM. It will show the host license information.

**Set DPI for the VM**

Added a new flag in `anka modify set display` command to set DPI for the VM.

```
anka modify set display [OPTIONS]
  configure displays
Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  --no-vnc               disable VNC access to the VM
  -d, --dpi INTEGER      set DPI (default is 72)
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
```

**Disable VNC access to the VM**

Added a new flag in `anka modify set display` command to disable VNC access for the VM.

```
anka modify set display [OPTIONS]
  configure displays
Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  --no-vnc               disable VNC access to the VM
  -d, --dpi INTEGER      set DPI (default is 72)
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
```



## What's New in Anka Version 2.2.1

**Set resolution display for the VM with `anka modify set display`.**

```
anka modify set display [OPTIONS]

  configure displays

Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
  ```

  



**Send virtual bound events programmatically. This can be used to automate bypass of Apple confirmation dialog.**

 ```
 anka view [OPTIONS] VM_ID

  Open VM display viewer

Options:
  -d, --display INTEGER  Specify the displays to view
  -s, --screenshot       Make png screenshot
  -c, --click TEXT       Send the pointer event
  --help                 Show this message and exit.
  ```


`anka view VM --click 100,200` will click virtual click event at location 100*200

***Note*** Also available through /Library/Application\ Support/Veertu/Anka/addons/click 100,200

  


