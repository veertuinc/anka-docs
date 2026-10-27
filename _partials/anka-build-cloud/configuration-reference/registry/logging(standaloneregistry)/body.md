
---
---
### Logging (standalone registry)
{{< include file="_partials/anka-build-cloud/configuration-reference/registry/logging(standaloneregistry)/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_ACCESS_LOGS | (boolean) | Enables registry access logs. | false |
| ANKA_CMD_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived CMD logs; 0 inherits log-max-days. | 0 |
| ANKA_CMD_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived CMD logs; 0 inherits log-max-mb. | 0 |
| ANKA_ENABLE_CENTRAL_LOGGING | (boolean) | Enables central logging. This will forward all logs available to the service into the registry's data directory using the REST API of the Registry. | false |
| ANKA_ERROR_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived ERROR logs; 0 inherits log-max-days. | 14 |
| ANKA_ERROR_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived ERROR logs; 0 inherits log-max-mb. | 512 |
| ANKA_FILES_DIR | (string) | The directory to store non-VM related files. | /files |
| ANKA_INFO_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived INFO logs; 0 inherits log-max-days. | 0 |
| ANKA_INFO_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived INFO logs; 0 inherits log-max-mb. | 0 |
| ANKA_KEEP_LOGS_FOR | (int) | Retention: delete centralized log files older than this many days. | 14 |
| ANKA_LOG_MAX_DAYS | (int) | Retention: maximum age in days for archived logs of all types unless otherwise defined. | 7 |
| ANKA_LOG_MAX_MB | (int) | Retention: maximum total size in MB for archived logs of all types unless otherwise defined. | 2048 |
| ANKA_LOG_SERVER_BACKEND_TYPE | (string) | The backend type to use for the log server. Only 'disk' backend is currently supported. | disk |
| ANKA_LOG_SERVER_ADDR | (string) | The address and port the registry will send logs to. Likely required if using https/tls and your cert only allows specific domains. This is useful if you have multiple registries and only one is used for logging centralization (format: "http[s]://address:[port]"). |  |
| ANKA_LOGS_DIR | (string) | The directory to store centralized log files (relative to ANKA_BASE_PATH and ANKA_FILES_DIR; defaults to /mnt/vol/files/central-logs). | /central-logs |
| ANKA_MAX_LOG_SIZE | (int) | Rotation: maximum size in MB for each centralized log file before it is rotated. | 1024 |
| ANKA_ROTATE_LOG_FILES_AT_MAX_FILE_SIZE | (boolean) | Rotation: start a new centralized log file when it reaches max-log-size. | true |
| ANKA_ROTATE_LOG_FILES_END_OF_DAY | (boolean) | Rotation: start a new centralized log file when the calendar day changes. | true |
| ANKA_V | (string)  | number for the log level verbosity | 0 |
| ANKA_VM_DIR_PATH | (string) | Set the path to put vm directory (relative to base) | vm_dir |
| ANKA_VM_LIST_CACHE_TTL | (duration) | Template list cache TTL | 30s |
| ANKA_VM_LIST_CONCURRENCY | (int) | Template list concurrency | 10 |
| ANKA_VMODULE | (string)  | comma-separated list of pattern=N settings for file-filtered logging | 0 |
