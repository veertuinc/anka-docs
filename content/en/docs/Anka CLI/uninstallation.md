---
date: 2019-12-12T00:00:00-00:00
title: "Uninstallation"
linkTitle: "Uninstallation"
weight: 7
description: >
  Steps to remove the Anka CLI
---

From the command line, execute the following command:

> Make sure all your VMs are stopped.

`sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`

```
/Library/Application\ Support/Veertu/Anka/tools/uninstall.sh --help
Usage: uninstall.sh [OPTIONS...]

Options:
-a|--along-with-machines	Also remove all Anka virtual machines (for all users) created and license information
-f|--force			Force mode (do not ask user confirmation)
-h|--help			Show this message
```

> Uninstall doesn't remove the license. To remove license execute `sudo anka license remove` before uninstalling. Make a note of the fulfillment id that is displayed in the output of `sudo anka license remove`.

### Moving license from one host to another

To move the license from an already activated mac to another mac, execute the following steps.

`sudo anka license remove`. Then, make a note of the fulfillment id shown in the output of this command. Contact Veertu support with the activation key and the fulfillment id.


