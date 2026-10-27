
---
---
### Logging
{{< include file="_partials/anka-build-cloud/configuration-reference/controller/logging/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_CMD_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived CMD logs; 0 inherits log-max-days.) | 0 |
| ANKA_CMD_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived CMD logs; 0 inherits log-max-mb.) | 0 |
| ANKA_ENABLE_CENTRAL_LOGGING | (boolean) | Enables central logging. This will forward all logs available to the service into the registry's data directory using the REST API of the Registry.) | false |
| ANKA_ENABLE_EVENT_LOGGING | (boolean) | (Enterprise Plus Only) Enables event logging. **They will show under the Controller's Logs section after the first instance is created.**) | false |
| ANKA_ERROR_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived ERROR logs; 0 inherits log-max-days.) | 14 |
| ANKA_ERROR_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived ERROR logs; 0 inherits log-max-mb.) | 512 |
| ANKA_EVENT_LOG_URL | (string) | (Enterprise Plus Only) The url to post events to in json format.) |  |
| ANKA_INFO_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived INFO logs; 0 inherits log-max-days.) | 0 |
| ANKA_INFO_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived INFO logs; 0 inherits log-max-mb.) | 0 |
| ANKA_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived logs of all types unless otherwise defined.) | 7 |
| ANKA_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived logs of all types unless otherwise defined.) | 2048 |
| ANKA_V | (string)  | verbosity level of logs (0-10, 10 being the most verbose)) | 0 |
