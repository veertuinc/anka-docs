
---
---
### Separate Queue Interface
{{< include file="content/_partials/Anka Build Cloud/configuration-reference/controller/separatequeueinterface/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_CLEAN_QUEUES_INTERVAL | (duration) | The interval to clean the queues (delete any tasks older than 24 hours), 0 to disable | 1h0m0s |
| ANKA_ENABLE_QUEUE_AUTH | (boolean) | Enable queue Authentication | false |
| ANKA_QUEUE_ADDR | (string) | The address to use for the queue (format: "0.0.0.0:[port]") |  |
| ANKA_QUEUE_CA_CERT | (string) | The HTTPS/TLS CA cert for the queue |  |
| ANKA_QUEUE_SERVER_CERT | (string) | The HTTPS/TLS certificate file in PEM format for the queue |  |
| ANKA_QUEUE_SERVER_KEY | (string) | The HTTPS/TLS private key in PEM format for the queue |  |
| ANKA_USE_QUEUE_TLS | (boolean) | Enable queue HTTPS/TLS | false |
