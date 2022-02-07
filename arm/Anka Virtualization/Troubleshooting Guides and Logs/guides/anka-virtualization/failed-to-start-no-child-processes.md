---
title: "failed to start: No child processes"
linkTitle: "failed to start: No child processes"
weight: 1
---

## Scenario

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

## Common reasons

1. 2.5 was installed previously.
2. You are logged into the machine with 1 user, then SSHing in as another.
3. Your anti-virus is blocking the processes from starting.

## Solutions

1. Re-install 2.5 and run `sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`.
2. Log into the UI with the user you're using for SSH.
3. Disable all anti-virus software.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
