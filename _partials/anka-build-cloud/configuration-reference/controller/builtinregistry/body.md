

### Built in Registry
{{< include file="_partials/anka-build-cloud/configuration-reference/controller/builtinregistry/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ENABLE_REGISTRY_AUTHORIZATION | (boolean) | Enable Authorization (Users, groups, permission control for specific certificates) in the Registry. | false |
| ANKA_REGISTRY_ACCESS_LOGS | (boolean) | Enables registry access logs. | false |
| ANKA_REGISTRY_BASE_PATH | (string) | Built-in Registry's data storage path. |  |
| ANKA_REGISTRY_LISTEN_ADDRESS | (string) | Address for built in Registry to listen on. | :8089 |
| ANKA_REGISTRY_VM_LIST_CACHE_TTL | (duration) | Registry's template information cache TTL | 30s |
| ANKA_RUN_REGISTRY | (boolean) | Run Built-in Registry (useful if not using standalone mode, but you still want the controller and registry to run together; no etcd). | false |
