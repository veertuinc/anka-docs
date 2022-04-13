---
date: 2022-04-01T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.23.0"
---

### Request a VM with a specific VLAN

Starting in this version [you can now set `vlan_tag` to the targeted VLAN ID in your Controller API calls]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#start-vm-instances" >}}).

### View available VLANs attached to a specific Host/Anka Node

You can also [list all VLAN IDs across all nodes in your fleet]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#get-all-vlans-ids" >}}).

```bash
curl http://anka.controller/api/v1/vlan
{
  "status": "OK",
  "message": "",
  "body": ["12", "55"]
}
```

### Batch processing of tasks

In former versions controllers would reserve tasks one by one.
To increase performance and lower network usage we added batch processing for tasks.
It's controlled by the config argument `batch-task-count` (defaults to 2).
The controller would take up that number of tasks and perform them in parallel.
There is a maximum of 40 due to etcd hard limit for the number of keys in a transaction.

We also increased the efficiency of reserving the tasks when there are multiple controllers taking tasks from the queue.

### Changes to workers

Instead of each worker trying to take tasks by itself, there is one process trying to reserve tasks from the queue continuously.

#### Why batch processing?

After looking at the rate of processing, we understand that most tasks are completed very quickly, but the action of reserving a task takes a lot of time.
By using the batch processing approach we increase the throughput of the requests but save a lot of time reserving the tasks.

#### New Stats

We added a new stat (under the `/api/v1/stats` endpoint) `tasks_per_second` that shows how many tasks are processed every second. It's possible to use that to calculate a more suitable heartbeat for the nodes.
For example, if a cloud has 20 nodes and has a heartbeat of 5 seconds we would expect tasks per second to be around 1.6 on an idle system.
If the number of tasks per second is lower than 1.6 that means that the system is not processing the tasks fast enough. While that could be from numerous reasons, increasing the heartbeat or the batch number could reduce load and even out the systems's performance.
