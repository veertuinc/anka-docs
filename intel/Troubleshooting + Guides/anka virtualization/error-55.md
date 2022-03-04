---
title: "VM log filled with error 55"
linkTitle: "VM log filled with error 55"
weight: 1
---

## Scenarios

- The `[~]/Library/Logs/Anka/{UUID}.log` is filled with `22: failed to write packet, error 55` and `failed to send # packets: error 55`

## Common Causes and solutions

1. The send buffer is exceeding. Normally it shouldn't be a critical error -- packet loss is being handled by the TCP stack of guest. But if the error rate is very high, it definitely could be a symptom of network problems happening in guests. If the occurrence of these errors is more than just a few log entries (10s every millisecond for example), you need to increase the values of `sysctl net.local.dgram.maxdgram` and `sysctl net.local.dgram.recvspace` on the host (with no VMs running) and then reboot it.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
