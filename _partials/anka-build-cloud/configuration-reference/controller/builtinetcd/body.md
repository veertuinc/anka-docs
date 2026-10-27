
---
---
### Built in ETCD
{{< include file="_partials/anka-build-cloud/configuration-reference/controller/builtinetcd/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ADVERTISE_CLIENT_URLS | (string) | Comma separated list of client urls for ETCD to advertise (macOS standalone mode only)) | http://127.0.0.1:2379 |
| ANKA_AUTO_COMPACTION_MODE | (string) | The ETCD auto compaction mode, ('periodic' or 'revision') (macOS standalone mode only)) | periodic |
| ANKA_AUTO_COMPACTION_RETENTION | (string) | The ETCD auto compaction retention length (0 is disabled) (macOS standalone mode only)) | 30m |
| ANKA_DATA_DIR | (string) | The ETCD data directory location (macOS standalone mode only)) | /tmp/etcd-data |
| ANKA_INITIAL_ADVERTISE_PEER_URLS | (string) | Comma separated list of peer urls for ETCD to advertise (macOS standalone mode only)) | http://0.0.0.0:2380 |
| ANKA_INITIAL_CLUSTER | (string) | The initial ETCD cluster configuration for bootstrapping (macOS standalone mode only)) | anka-etcd=http://0.0.0.0:2380 |
| ANKA_INITIAL_CLUSTER_STATE | (string) | The initial cluster state for ETCD ('new' or 'existing') (macOS standalone mode only)) | new |
| ANKA_INITIAL_CLUSTER_TOKEN | (string) | The cluster token used in ETCD during bootstrap (macOS standalone mode only)) | etcd-server |
| ANKA_LISTEN_CLIENT_URLS | (string) | Comma separated list client urls for ETCD to use (macOS standalone mode only)) | http://127.0.0.1:2379 |
| ANKA_LISTEN_PEER_URLS | (string) | Comma separated list of peer urls for ETCD to use (macOS standalone mode only)) | http://0.0.0.0:2380 |
| ANKA_NAME | (string) | The name for your ETCD server (macOS standalone mode only)) | anka-etcd |
