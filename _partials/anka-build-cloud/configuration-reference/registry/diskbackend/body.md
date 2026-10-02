
---
---
### Disk backend
{{< include file="_partials/anka-build-cloud/configuration-reference/registry/diskbackend/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BACKEND_DISK_LOG_SLOW_IO_TIMEOUT | (duration) | Log disk backend read and write operations that remain blocked for this duration. Zero disables slow I/O logging. | 0 |
| ANKA_BACKEND_DISK_UPLOAD_STALL_ABORT_AFTER_UNBLOCK | (duration) | Mark an upload failed after a disk write unblocks if that write was blocked longer than this duration. The active write cannot be interrupted. Zero disables the watchdog. | 0 |
