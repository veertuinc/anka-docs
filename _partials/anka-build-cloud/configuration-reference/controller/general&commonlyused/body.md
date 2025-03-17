
---
---
### General & Commonly used
{{< include file="content/_partials/anka-build-cloud/configuration-reference/controller/general&commonlyused/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ANKA_REGISTRY | (string) | The Anka Registry address used for communication from the Controller to the Registry as well as the address used by Nodes to pull VM Templates and Tags.) |  |
| ANKA_CLEAN_MAC_ADDRESS_INTERVAL | (duration) | Delay between cleaning mac addresses.) | 1h0m0s |
| ANKA_DEFRAG_DB_INTERVAL | (duration) | The interval for defragging ETCD (0 is disable).) | 0 |
| ANKA_ETCD_ENDPOINTS | (string)  | 	Comma separated list of etcd addresses. These endpoints are used for the Application DB (instance, group, node information) and the Queue DB (if not defined separately with ANKA_QUEUE_ETCD_ENDPOINTS).) | 127.0.0.1:2379 |
| ANKA_FILL_MAC_ADDRESS_RANGE_INTERVAL | (duration) | Interval to execute the mac address range validation.) | 3h0m0s |
| ANKA_INSTANCE_TIME_OUT | (duration) | The time that instances stay in 'Terminated' or 'Terminating' state.) | 1m0s |
| ANKA_LISTEN_ADDR | (string) | The address and port to listen on (format: [address]:port).) | :80 |
| ANKA_LOCAL_ANKA_REGISTRY | (string) | Do not set unless ANKA_ANKA_REGISTRY is set to a URL/IP the controller does not have access to. The Controller uses this to communicate with the Registry and is separate from the ANKA_ANKA_REGISTRY, which is used by external services like Anka Nodes. This is for situations where the Controller and Registry are on the same network and you want to use localhost/local DNS for communication between them (format: http[s]://address:[port]).) |  |
| ANKA_MAC_ADDR_RANGE | (string) | Pass the range of mac addresses to use. manage-mac-addresses must be set to true to use this option. format is <FROM>-<TO> (example: 00:00:00:00:00:00-FF:FF:FF:FF:FF:FF).) |  |
| ANKA_MAC_ADDR_RANGE_MAX_RETRIES | (int) | Times to retry to get mac address from the database before giving up and returning an error.) | 100 |
| ANKA_MANAGE_MAC_ADDRESSES | (boolean) | Enables the controller to manage mac addresses of VMs. Check our docs for more info and caveats.) | false |
| ANKA_NUM_WORKERS | (int) | The number of concurrent workers processing node tasks.) | 2 |
| ANKA_PUSH_REGISTRY | (string)  | Comma separated list of Registry addresses to use for push operations (saveImage/Jenkins cache building).) |  |
| ANKA_QUEUE_ETCD_ENDPOINTS | (string)  | Comma seperated list of ETCD endpoints to use for queue data (only available in standalone mode).) |  |
| ANKA_STANDALONE | (boolean) | Run controller service with built etcd database in a single binary/service.) | false |
