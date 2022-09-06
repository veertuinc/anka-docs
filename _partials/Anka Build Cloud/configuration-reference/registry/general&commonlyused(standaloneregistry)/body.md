
---
---
### General & Commonly used (standalone registry)
{{< include file="content/_partials/Anka Build Cloud/configuration-reference/registry/general&commonlyused(standaloneregistry)/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_INTERNAL_LISTEN_ADDR | (string) | The secondary address and port to listen on. This is for situations where the Controller and Registry are on the same network and you want to use localhost/local DNS for communication between them (format: "http[s]://address:[port]"). |  |
| ANKA_LISTEN_ADDR | (string) | The address and port to listen on (format: "http[s]://address:[port]"). |  |
| ANKA_VM_LIST_CACHE_TTL | (duration) | Template information cache TTL | 30s |
