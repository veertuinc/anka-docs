
---
---
### Monitoring
{{< include file="content/_partials/anka-build-cloud/configuration-reference/controller/monitoring/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ENABLE_METRICS | (boolean) | Enables Prometheus metrics. By default available on *:2112/metrics) | false |
| ANKA_METRICS_PATH | (string) | Path to expose Prometheus metrics.) | /metrics |
| ANKA_METRICS_PORT | (uint) | Port to expose Prometheus metrics.) | 2112 |
