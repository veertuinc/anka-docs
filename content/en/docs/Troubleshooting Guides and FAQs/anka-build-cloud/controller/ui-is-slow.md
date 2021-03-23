---
title: "UI is slow to load"
linkTitle: "UI is slow to load"
weight: 1
---

## Scenario

Loading the UI seems to be very slow. Node status, CPU, etc, all seem to be delayed.

## Common reasons

1. Lots of VMS, Nodes, and not enough load balancing/instances of the controller, etcd, etc.

## Solutions:

1. Increase the space quota from the default 2GB: [https://etcd.io/docs/current/op-guide/maintenance/#space-quota]
2. Rejoin your nodes with a higher `--heartbeat` value

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)