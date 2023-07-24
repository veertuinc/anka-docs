---
title: "maximum number of running instances exceeded"
weight: 1
---

## Scenario

On Apple Silicon / `arm64`, you can sometimes see `maximum number of running instances exceeded`:

```bash
❯ grep -E "(startVMRequest|maximum)" /Users/nathanpierce/Downloads/anka-node-diagnostics-root\ 11/var/log/veertu/anka_agent.INFO
I0502 00:03:24.177527    7961 start_vm.go:51] started handling startVMRequest
I0502 00:03:27.263930    7961 start_vm.go:51] started handling startVMRequest
I0502 01:03:25.553619    7961 start_vm.go:51] started handling startVMRequest
I0502 01:17:07.023350    7961 start_vm.go:51] started handling startVMRequest
I0502 01:46:57.254228    7961 start_vm.go:51] started handling startVMRequest
I0502 01:50:56.817503    7961 start_vm.go:51] started handling startVMRequest
I0502 01:53:15.330025    7961 start_vm.go:51] started handling startVMRequest
I0502 01:58:35.581537    7961 start_vm.go:51] started handling startVMRequest
E0502 01:58:47.233518    7961 arm_v1.go:98] failed to start vm 326d5bab-acb0-4fe9-b6a5-054b47afe720: 326d5bab-acb0-4fe9-b6a5-054b47afe720: maximum number of running instances exceeded
E0502 01:58:48.313380    7961 start_vm.go:196] requestId: 94f4a0cb-8649-45a7-5a6b-bb56e5562201, instance id: 34217219-05cf-47f8-7aef-b5641cd7d2cb, error: 326d5bab-acb0-4fe9-b6a5-054b47afe720: maximum number of running instances exceeded
E0502 01:58:48.810655    7961 start_vm.go:59] error handling start vm request: 326d5bab-acb0-4fe9-b6a5-054b47afe720: maximum number of running instances exceeded
I0502 01:58:48.810694    7961 start_vm.go:70] Not releasing task 94f4a0cb-8649-45a7-5a6b-bb56e5562201 back to queue due to error: 326d5bab-acb0-4fe9-b6a5-054b47afe720: maximum number of running instances exceeded
I0502 02:00:00.481464    7961 start_vm.go:51] started handling startVMRequest
I0502 02:59:28.044878    7961 start_vm.go:51] started handling startVMRequest
```

## Solution

At the moment this seems to be a problem with Apple's virtualization. If you ever see this again, please run `sudo sysdiagnose -u` and then email the sysdiagnose file that appears in `/private/var/tmp/` to support@veertu.com.

The issue **should** resolve itself automatically given enough time. If not, a reboot of the host will solve it.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

