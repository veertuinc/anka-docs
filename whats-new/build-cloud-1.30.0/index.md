---
date: 2022-12-01T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.30.1"
---

### Ability to set `--local` and `--no-local`

In this version, you can now set `--local` and `--no-local` when starting VMs. This will stop the VM before performing the modify command, if not already stopped. You can read more in the [Working with the Controller and API guide]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#start-vm-instances" >}})

{{< include file="_partials/anka-virtualization-cli/modify/network/_index.md" >}}

```shell
 curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
  -d '{"vmid": "6b135004-0c89-43bb-b892-74796b8d266c", "count": 2, "network_local": "forceOn" }'

{
  "status": "OK",
  "message": "",
  "body": [
    "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce",
    "e74dfc0e-dc94-4ca2-575e-3219ac08ffa2"
  ]
}
```
