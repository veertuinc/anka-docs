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

## Common reasons

1. 2.5 was installed previously.
2. You are logged into the machine with 1 user, then SSHing in as another.

## Solutions

1. Re-install 2.5 and run `sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`.
2. Log into the UI with the user you're using for SSH.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
