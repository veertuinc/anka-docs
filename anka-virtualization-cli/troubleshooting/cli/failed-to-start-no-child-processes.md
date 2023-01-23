---
title: "failed to start: No child processes"
linkTitle: "failed to start: No child processes"
weight: 1
---

## Scenarios

```bash
anka --debug start -uv 12.0.1
3fcc268d-ac62-4951-8e64-32b350530c56: failed to start: status -43
3fcc268d-ac62-4951-8e64-32b350530c56: failed to start: error 70
3fcc268d-ac62-4951-8e64-32b350530c56: hypervisor failed with status 70
anka: 12.0.1: failed to start: No child processes
```

```bash
usr@machine % anka --debug start 12.2.0-arm
Thu Feb  3 15:32:21 main: executing command start
Thu Feb  3 15:32:21 vm: a25d8ea6-73a9-4921-be75-c25f930d2934: working directory: /Users/usr/Library/Application Support/Veertu/Anka/vm_lib/a25d8ea6-73a9-4921-be75-c25f930d2934
Thu Feb  3 15:32:21 vm: a25d8ea6-73a9-4921-be75-c25f930d2934: job started with pid 94454
utils: 94454: failed to get proc state: No such file or directory
Thu Feb  3 15:32:22 vm: a25d8ea6-73a9-4921-be75-c25f930d2934: process early exit status 256
Thu Feb  3 15:32:22 vm: a25d8ea6-73a9-4921-be75-c25f930d2934: bad VM start status: No such file or directory
Thu Feb  3 15:32:22 start: a25d8ea6-73a9-4921-be75-c25f930d2934: failed to start: error 256
Thu Feb  3 15:32:22 start: a25d8ea6-73a9-4921-be75-c25f930d2934: hypervisor failed with status 256
anka: 12.2.0-arm: failed to start: No child processes
```

```bash
❯ cat /var/log/veertu/anka_agent.INFO
. . .
I0123 17:57:08.211474   15646 arm_v1.go:35] starting vm start process. request id: 97cc6147-9a21-431d-7c37-564d637d6c77, source vm: c3c080f3-5fd2-49f7-a7b9-356de3208b91
I0123 17:57:08.254469   15646 intel_v1.go:172] cloning c3c080f3-5fd2-49f7-a7b9-356de3208b91 with name mgmtManaged-Mactest12-1674493028254448000
E0123 17:57:08.444507   15646 arm_v1.go:98] failed to start vm 9b0459c9-d8f3-41a8-a355-cac8df9ee314: 9b0459c9-d8f3-41a8-a355-cac8df9ee314: failed to start: No child processes
. . .
```

## Common Causes

1. Port forwarding is conflicting with an already existing VM:
    ```
    Mon Jan 23 17:57:08 pid 43390: session launching...
    port_fwd: failed to bind 0.0.0.0, port 59000: Address already in use
    port_fwd: listen(-1): Bad file descriptor
    ankahv: failed to add rule tcp:59000:0.0.0.0:5900, error 9
    ankahv: failed to start: Bad file descriptor
    ```
1. Intel Anka version was installed on ARM and then not fully uninstalled when ARM version was obtained and installed.
2. You are logged into the machine with 1 user, then SSHing in as another.
3. Your anti-virus is blocking the processes from starting.

## Solutions

1. Re-install 2.5 and run `sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`.
2. Log into the UI with the user you're using for SSH.
3. Disable all anti-virus software.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
