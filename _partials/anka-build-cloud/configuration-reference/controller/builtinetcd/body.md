
---
---
### Built in ETCD
{{< include file="content/_partials/anka-build-cloud/configuration-reference/controller/builtinetcd/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ADVERTISE_CLIENT_URLS | (string) | Comma separated list of client urls for ETCD to advertise (only available in standalone mode) | http://127.0.0.1:2379 |
| ANKA_AUTO_COMPACTION_MODE | (string) | The ETCD auto compaction mode, ('periodic' or 'revision') (only available in standalone mode) | periodic |
| ANKA_AUTO_COMPACTION_RETENTION | (string) | The ETCD auto compaction retention length (0 is disabled) (only available in standalone mode) | 30m |
| ANKA_DATA_DIR | (string) | The ETCD data directory location (only available in standalone mode) | /tmp/etcd-data |
| ANKA_INITIAL_ADVERTISE_PEER_URLS | (string) | Comma separated list of peer urls for ETCD to advertise (only available in standalone mode) | http://0.0.0.0:2380 |
| ANKA_INITIAL_CLUSTER | (string) | The initial ETCD cluster configuration for bootstrapping (only available in standalone mode) | anka-etcd=http://0.0.0.0:2380 |
| ANKA_INITIAL_CLUSTER_STATE | (string) | The initial cluster state for ETCD ('new' or 'existing') (only available in standalone mode) | new |
| ANKA_INITIAL_CLUSTER_TOKEN | (string) | The cluster token used in ETCD during bootstrap (only available in standalone mode) | etcd-server |
| ANKA_LISTEN_CLIENT_URLS | (string) | Comma separated list client urls for ETCD to use (only available in standalone mode) | http://127.0.0.1:2379 |
| ANKA_LISTEN_PEER_URLS | (string) | Comma separated list of peer urls for ETCD to use (only available in standalone mode) | http://0.0.0.0:2380 |
| ANKA_NAME | (string) | The name for your ETCD server (only available in standalone mode) | anka-etcd |
