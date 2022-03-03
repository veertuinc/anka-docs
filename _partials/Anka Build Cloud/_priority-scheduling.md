---
---

When requesting multiple VMs through the API, a priority can be assigned. The lower the priority integer, the higher the urgency:

```bash
# Request a VM with the highest priority (default priority is 1000)
curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
  -d '{"vmid": "6b135004-0c89-43bb-b892-74796b8d266c", "count": 2, "priority": 1}'

{
  "status": "OK",
  "message": "",
  "body": [
    "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce",
    "e74dfc0e-dc94-4ca2-575e-3219ac08ffa2"
  ]
}
```

{{< hint info >}}
By default, nodes with the targeted Template ("vmid") on them receive a higher priority compared with those that do not have the Template yet. This can make it seem as if some nodes are used more than others. We recommend distributing the Template to every node if this is a problem.
{{< /hint >}}
