

### Logging (standalone registry)
{{< include file="_partials/anka-build-cloud/configuration-reference/registry/logging(standaloneregistry)/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ACCESS_LOGS | (boolean) | Enables registry access logs. | false |
| ANKA_CMD_LOG_MAX_DAYS | (int) | Number of days to keep cmd logs (0 will use the value in log-max-days). | 7 |
| ANKA_CMD_LOG_MAX_MB | (int) | MB limit for cmd log files (0 will use the value in log-max-mb). | 1024 |
| ANKA_ENABLE_CENTRAL_LOGGING | (boolean) | Enables central logging. This will forward all logs available to the service into the registry's data directory using the REST API of the Registry. | false |
| ANKA_ERROR_LOG_MAX_DAYS | (int) | Number of days to keep error logs (0 will use the value in log-max-days). | 3 |
| ANKA_ERROR_LOG_MAX_MB | (int) | MB limit for error log files (0 will use the value in log-max-mb). | 200 |
| ANKA_FILES_DIR | (string) | The directory to store non-VM related files. | /files |
| ANKA_INFO_LOG_MAX_DAYS | (int) | Number of days to keep info logs (0 will use the value in log-max-days). | 0 |
| ANKA_INFO_LOG_MAX_MB | (int) | MB limit for info log files  (0 will use the value in log-max-mb). | 0 |
| ANKA_KEEP_LOGS_FOR | (int) | The number of days to keep individual centralized logs. | 7 |
| ANKA_LOG_MAX_DAYS | (int) | Number of days to keep logs for all log types unless otherwise defined. | 7 |
| ANKA_LOG_MAX_MB | (int) | MB limit for log files, for all log types unless otherwise defined. | 700 |
| ANKA_LOG_SERVER_BACKEND_TYPE | (string) | The log server backend type, either 'disk' or 'azure'. | disk |
| ANKA_LOG_SERVER_ADDR | (string) | The address and port the registry will send logs to. This is useful if you have multiple registries and only one is used for logging centralization (format: "http[s]://address:[port]"). |  |
| ANKA_LOGS_DIR | (string) | The directory to store centralized log files (relative to files-dir). | /central-logs |
| ANKA_MAX_LOG_SIZE | (int) | The maximum size for a centralized log file in MB. | 1024 |
| ANKA_ROTATE_LOG_FILES_AT_MAX_FILE_SIZE | (boolean) | Enable rotatation of centralized log files when they reach the size specified in max-log-size. | true |
| ANKA_ROTATE_LOG_FILES_END_OF_DAY | (boolean) | Enable rotatation of centralized log files at the end of each day. | false |
