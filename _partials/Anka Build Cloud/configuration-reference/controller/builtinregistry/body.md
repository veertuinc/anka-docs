
---
---
### Built in Registry
{{< include file="content/_partials/Anka Build Cloud/configuration-reference/controller/builtinregistry/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ENABLE_CONTROLLER_AUTHORIZATION | (boolean) | Enable Authorization for the Controller. | false |
| ANKA_ENABLE_REGISTRY_AUTHORIZATION | (boolean) | Enable Authorization for the built in Registry. | false |
| ANKA_REGISTRY_ACCESS_LOGS | (boolean) | Enables registry access logs. | false |
| ANKA_REGISTRY_BASE_PATH | (string) | Built-in Registry's data storage path. |  |
| ANKA_REGISTRY_LISTEN_ADDRESS | (string) | Address for built in Registry to listen on. | :8089 |
| ANKA_RUN_REGISTRY | (boolean) | Run Built-in Registry (useful if not using standalone mode, but you still want the controller and registry to run together; no etcd). | false |
