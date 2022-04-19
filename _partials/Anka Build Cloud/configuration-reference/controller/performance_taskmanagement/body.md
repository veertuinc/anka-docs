
---
---
### Performance / Task Management
{{< include file="content/_partials/Anka Build Cloud/configuration-reference/controller/performance_taskmanagement/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BATCH_TASK_COUNT | (int) | The number of tasks to get from the queue in one request (max 40) | 2 |
| ANKA_ETCD_REQUEST_TIMEOUT | (duration) | Client side timeout for ETCD requests | 20s |
| ANKA_INSTANCE_ACTIVE_TIMEOUT | (duration) | How long before an instance is declared as 'not communicating' | 2m0s |
| ANKA_NODE_ACTIVE_TIMEOUT | (duration) | How long before a node is declared as 'offline' | 2m0s |
| ANKA_SCHEDULER_INTERVAL | (duration) | The interval for checking scheduled tasks | 30m0s |
