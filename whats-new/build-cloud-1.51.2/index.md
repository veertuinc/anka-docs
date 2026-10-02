---
date: 2026-10-02T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.51.2"
---

### Registry upload write-stall watchdog {#registry-upload-write-stall-watchdog}

When a Registry VM is writing a large upload to disk, the Registry will watch for write stalls and restart the upload if it detects a stall.

| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BACKEND_DISK_UPLOAD_STALL_ABORT_AFTER_UNBLOCK | (duration) | Mark an upload failed after a disk write unblocks if that write was blocked longer than this duration. The active write cannot be interrupted. Zero disables the watchdog. | 0 |

### Graceful shutdown cleanup limit {#graceful-shutdown-cleanup-limit}

When the Registry receives a SIGINT or SIGTERM signal, it will start a graceful shutdown process. During this process, the Registry will track active HTTP requests and close them after a configurable cleanup period.

| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_FORCED_SHUTDOWN_CLEANUP_LIMIT | (duration) | The duration to wait for canceled requests to clean up after graceful shutdown times out | 8s |
| ANKA_GRACEFUL_SHUTDOWN_TIMEOUT | (duration) | The duration to wait for graceful shutdown when receiving SIGINT/SIGTERM | 2s |

### Slow IO detection during uploads {#slow-io-detection-during-uploads}

The Registry will detect and log slow IO during uploads.

| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BACKEND_DISK_LOG_SLOW_IO_TIMEOUT | (duration) | Log disk backend read and write operations that remain blocked for this duration. Zero disables slow I/O logging. | 0 |

Each detected slow operation now emits two warnings:
- `slow I/O read has been blocked for at least 5s`
- `slow I/O read unblocked after 12.34s`

Writes behave identically. Both `0s` and `0` will disable it.