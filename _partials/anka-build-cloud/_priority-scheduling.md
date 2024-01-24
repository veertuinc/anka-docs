

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
Nodes are constantly asking for tasks in the Controller. They will send a payload with information about itself to the Controller including the Templates/Tags it has, usb devices available, etc, and the Controller will then use that information to score and choose the best task for the node. The `priority` of the start VM request also contributes to this score. This is not a pure FIFO task queue though.

If nodes that do not have the Template are getting tasks and having to pull all the time, you should consider using groups and placing these nodes in a fallback group. Then, tell your jobs to target the non-fallback group and they will prioritize the nodes that have the templates.
{{< /hint >}}
