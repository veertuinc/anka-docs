---
date: 2019-12-12T00:00:00-00:00
title: "Uninstalling (Anka)"
linkTitle: "Uninstalling"
weight: 6
description: >
  Steps for uninstalling the Anka Virtualization package
---

{{< hint info >}}
Uninstall doesn't remove the license. To remove license execute `sudo anka license remove` before uninstalling. Make a note of the fulfillment id!
{{< /hint >}}

From the command line, execute the following command. Make sure all your VMs are stopped.

`sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`

```bash
/Library/Application\ Support/Veertu/Anka/tools/uninstall.sh --help
Usage: uninstall.sh [OPTIONS...]

Options:
-a|--along-with-machines	Also remove all Anka virtual machines (for all users) created and license information
-f|--force			Force mode (do not ask user confirmation)
-h|--help			Show this message
```

### Moving license from one host to another

To move the license from an already activated mac to another mac, execute the following steps.

`sudo anka license remove`. Then, make a note of the fulfillment id shown in the output of this command. Contact Veertu support with the activation key and the fulfillment id.
