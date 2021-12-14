---
title: "No such file or directory in anka_agent logs"
linkTitle: "No such file or directory in anka_agent logs"
weight: 1
---

## Scenario

If you have not created or run a VM before on a machine, and try to pull a VM from the registry through the Anka Agent, you'll see an error in the agent logs like:

```bash
I1214 22:57:11.857070     615 start_vm.go:217] checking for disk space
E1214 22:57:12.714660     615 start_vm.go:44] [Errno 2] No such file or directory: '/var/root/Library/Application Support/Veertu/Anka/img_lib/'
I1214 22:57:12.725765     615 start_vm.go:36] started handling startVMRequest
```

## Solution

Run `sudo anka create test && sudo anka delete --yes test` at least once on the machine and try again.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

