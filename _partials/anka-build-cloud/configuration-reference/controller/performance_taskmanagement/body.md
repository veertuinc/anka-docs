

### Performance / Task Management
{{< include file="_partials/anka-build-cloud/configuration-reference/controller/performance_taskmanagement/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BATCH_TASK_COUNT | (int) | The number of tasks to get from the queue in one request (max 40) | 2 |
| ANKA_DIAL_TIMEOUT | (duration) | set http dial timeout | 5s |
| ANKA_ETCD_REQUEST_TIMEOUT | (duration) | Client side timeout for ETCD requests | 20s |
| ANKA_INSTANCE_ACTIVE_TIMEOUT | (duration) | How long before an instance is declared as 'not communicating' | 2m0s |
| ANKA_MAX_IDLE_CONNECTION_PER_HOST | (int) | set mac idle connections per host | 50 |
| ANKA_NODE_ACTIVE_TIMEOUT | (duration) | How long before a node is declared as 'offline' | 2m0s |
| ANKA_NUM_HTTP_RETRIES | (int) | Number of times to retry on http error > 400 | 5 |
| ANKA_QUERY_TASK_TIMEOUT | (duration) | Seconds nodes wait to reserve a start vm task if queue is empty | 10s |
| ANKA_REQUEST_TIMEOUT | (duration) | set http request timeout | 15s |
| ANKA_RESERVE_TASK_TIMEOUT | (duration) | Seconds queue clients wait to reserve a task if queue is empty | 10s |
| ANKA_SCHEDULER_INTERVAL | (duration) | The interval for checking scheduled tasks | 30m0s |
| ANKA_TLS_HANDSHAKE_TIMEOUT | (duration) | set tls handshake timeout | 5s |
| ANKA_UNKNOWN_VM_THRESHOLD | (int) | Number of reports allowed for an unknown VM before terminating it | 30 |
