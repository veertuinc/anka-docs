

Starting in 1.32.0, users can now use the **Change Node Configuration** feature through the UI to control Capacity as well as change the Anka Node to **Drain Mode**. This will allow VMs to finish their jobs, but not accept any new VM start tasks.

![change node config]({{< siteurl >}}images/whatsnew/1.32.0/change-node-config.png)
![drain mode]({{< siteurl >}}images/whatsnew/1.32.0/drain-mode.png)

You can also use the API: 

```bash
❯ curl -X POST http://anka.controller:8090/api/v1/node/config -d '{"node_id": "3c101836-9540-4733-9482-604d0c5fbe30", "drain_mode": true}'
{"status":"OK","message":""}
```