
---
date: 2019-12-12T00:00:00-00:00
title: "Anka USB Commands"
linkTitle: "USB Command Reference"
weight: 5
description: >
  Commands for working with USB devices.  
---



## Claiming, listing and releasing USB attached devices on Anka Hosts/nodes

```
anka usb --help
Usage: anka usb [OPTIONS] COMMAND [ARGS]...

  Do actions on USB devices

Options:
  --help  Show this message and exit.

Commands:
  claim    make a device(s) available for attaching to a...
  list     list available usb devices
  release  release a device(s) back to host availability
```

## Claiming devices with same claim name, -n flag, to create a virtual group across multiple hosts/nodes

```
anka usb claim --help
Usage: anka usb claim [OPTIONS] [DEVID]...

  make a device(s) available for attaching to a vm

Options:
  -n, --claim-name TEXT  claim name could be used as additional name
  --help                 Show this message and exit.
```

## Starting a VM with device attached at runtime using the -d flag

```
anka start --help
Usage: anka start [OPTIONS] VM_ID

  Starts or resumes paused VM

Options:
  -u, --update-addons, --update  Run guest addons update procedure, previous
                                 version of the addons should be already
                                 installed
  -f, --force                    Start VM with minimum checks
  -v, --view                     Open display window
  -o, --optical-drive TEXT       Path to ISO file or device
  -d, --usb TEXT                 USB device ID/Location (should be claimed)
  --help                         Show this message and exit.
```


