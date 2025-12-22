---
title: "ETCD logs say \"etcdserver: no space\""
linkTitle: "ETCD logs say \"etcdserver: no space\""
weight: 1
---

## Scenario

Sometimes after upgrading ETCD from an older verison, you may see logs like this:

```
<ttl:75-second id:6fb69b4556a03284>","response":"","error":"etcdserver: no space"}
{"level":"warn","ts":"2025-12-22T10:19:34.237788Z","caller":"etcdserver/util.go:123","msg":"failed to apply request","took":"3.068µs","request":"header:<ID:8049792106079728261 > lease_grant:<ttl:75-second id:6fb69b4556a03284>","response":"","error":"etcdserver: no space"}
{"level":"warn","ts":"2025-12-22T10:19:34.237441Z","caller":"etcdserver/util.go:123","msg":"failed to apply request","took":"3.715µs","request":"header:<ID:8049792106079728261 > lease_grant:<ttl:75-second id:6fb69b4556a03284>","response":"","error":"etcdserver: no space"}
```

## Common Causes

1. The backend database may exhibit internal fragmentation over time due to compaction.

## Solution

https://etcd.io/docs/v3.5/op-guide/maintenance/#defragmentation

Note: It is not safe to perform on an in-use Anka Build Cloud. Please put your cluster into maintenance mode before performing defragmentation.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

