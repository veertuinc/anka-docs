---
date: 2024-01-24T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.41.0"
---

### Ability to set External ID when creating a new VM Instance from the UI

You can now set the External ID for VM Instances. This helps with identifying Instances in the list which are not related to your automation/CI.

{{< imgwithlink src="images/whatsnew/1.41.0/vm-instance-creation-external-id.png" >}}

### Ability to join a Node in "Drain Mode"

```bash
❯ sudo ankacluster join --help
Joins the current machine to one or many Anka Build Cloud Controllers

Usage:
  ankacluster join CONTROLLER_ADDRESS[,CONTROLLER_ADDRESS2] [flags]

Flags:
. . .
      --drain-mode                            Join node with Drain Mode
. . .

```

### VM Instances listing in UI now shows Creation Time + sortable by Creation Time

The Controller UI's VM Instances list now indicates the Creation Time of a VM Instance. You can also sort the listing based on Creation Time.

{{< imgwithlink src="images/whatsnew/1.41.0/instances-list-creation-date.png" >}}
