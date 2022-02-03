---
title: "ETCD logs say \"database space exceeded\""
linkTitle: "ETCD logs say \"database space exceeded\""
weight: 1
---

## Scenario

Anka VMs error or disappear and you see in the Controller logs `E1108 23:09:28.804391       1 queue_server.go:184] Server Internal error: etcdserver: mvcc: database space exceeded.`. There was plenty of available space on disk. Running `etcdctl compact $(ETCDCTL_API=3 etcdctl --endpoints=:2379 endpoint status --write-out="json" | egrep -o '"revision":[0-9]*' | egrep -o '[0-9].*')`, `etcdctl debug --command-timeout=20s`, followed by `etcdctl alarm disarm` fixes it, but you'd like to prevent this in the future.

## Common reasons

1. Lots of VMS, Nodes, and tasks

## Solution

Increase the space quota from the default 2GB: https://etcd.io/docs/v3.4/op-guide/maintenance/#space-quota

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

