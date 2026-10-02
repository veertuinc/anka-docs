---
---

{{< hint warning >}}
Registry uploads write VM templates to the disk backend. Kubernetes volumes, Amazon EBS, and Amazon EFS can stall those writes. Set `ANKA_BACKEND_DISK_LOG_SLOW_IO_TIMEOUT` to log reads and writes that stay blocked. Set `ANKA_BACKEND_DISK_UPLOAD_STALL_ABORT_AFTER_UNBLOCK` to fail an upload after a blocked write unblocks. Both values default to `0`. A value of `0` disables the setting.
{{< /hint >}}
