---
date: 2020-01-27
title: "What's New"
linkTitle: "What's New"
weight: 12
description: >
  Published February 24, 2020 
---

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

  


