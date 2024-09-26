
---
---
### Logging
{{< include file="content/_partials/anka-build-cloud/configuration-reference/controller/logging/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_CMD_LOG_MAX_DAYS | (int) | Number of days to keep cmd logs (0 will use the value in log-max-days).) | 7 |
| ANKA_CMD_LOG_MAX_MB | (int) | MB limit for cmd log files (0 will use the value in log-max-mb).) | 1024 |
| ANKA_ENABLE_CENTRAL_LOGGING | (boolean) | Enables central logging. This will forward all logs available to the service into the registry's data directory using the REST API of the Registry.) | false |
| ANKA_ENABLE_EVENT_LOGGING | (boolean) | (Enterprise Plus Only) Enables event logging. **They will show under the Controller's Logs section after the first instance is created.**) | false |
| ANKA_ERROR_LOG_MAX_DAYS | (int) | Number of days to keep error logs (0 will use the value in log-max-days).) | 3 |
| ANKA_ERROR_LOG_MAX_MB | (int) | MB limit for error log files (0 will use the value in log-max-mb).) | 200 |
| ANKA_EVENT_LOG_URL | (string) | (Enterprise Plus Only) The url to post events to in json format.) |  |
| ANKA_INFO_LOG_MAX_DAYS | (int) | Number of days to keep info logs (0 will use the value in log-max-days).) | 0 |
| ANKA_INFO_LOG_MAX_MB | (int) | MB limit for info log files  (0 will use the value in log-max-mb).) | 0 |
| ANKA_LOG_MAX_DAYS | (int) | Number of days to keep logs for all log types unless otherwise defined.) | 7 |
| ANKA_LOG_MAX_MB | (int) | MB limit for log files, for all log types unless otherwise defined.) | 700 |
| ANKA_V | (string)  | verbosity level of logs (0-10, 10 being the most verbose)) | 0 |
