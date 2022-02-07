---
title: "Controller unable to start: pgid (######) above high water mark"
linkTitle: "Controller unable to start: pgid (######) above high water mark"
weight: 1
---

## Scenario

An error seen in the controller logs and blocks it starting up:

```text
2022-01-03 03:06:33.953508 E | etcdserver: cannot monitor file descriptor usage (cannot get FDUsage on darwin)
2022-01-03 03:06:33.953688 I | etcdserver: a51704efaf54bd03 as single-node; fast-forwarding 9 ticks (election ticks 10)
panic: pgid (8388923) above high water mark (3340)

goroutine 132 [running]:
github.com/coreos/bbolt.(*node).spill(0xc001240d90, 0xc002b57ba0, 0x5)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/bbolt/node.go:375 +0x6a5
github.com/coreos/bbolt.(*Bucket).spill(0xc00126a558, 0xe1a29b8, 0x3f1a9a0)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/bbolt/bucket.go:568 +0x473
github.com/coreos/bbolt.(*Tx).Commit(0xc00126a540, 0xe19d8e0, 0x3f1a9a0)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/bbolt/tx.go:160 +0xec
github.com/coreos/etcd/mvcc/backend.(*batchTx).commit(0xc001421230, 0x0)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/etcd/mvcc/backend/batch_tx.go:167 +0x7c
github.com/coreos/etcd/mvcc/backend.(*batchTxBuffered).unsafeCommit(0xc001421230, 0xc001314700)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/etcd/mvcc/backend/batch_tx.go:239 +0x49
github.com/coreos/etcd/mvcc/backend.(*batchTxBuffered).commit(0xc001421230, 0xc001314700)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/etcd/mvcc/backend/batch_tx.go:227 +0x4c
github.com/coreos/etcd/mvcc/backend.(*batchTxBuffered).Commit(0xc001421230)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/etcd/mvcc/backend/batch_tx.go:214 +0x42
github.com/coreos/etcd/mvcc/backend.(*backend).run(0xc001442150)
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/etcd/mvcc/backend/backend.go:273 +0xb9
created by github.com/coreos/etcd/mvcc/backend.newBackend
        /Users/anka/workspace/cloud-build_release_v1.18.0/src/github.com/coreos/etcd/mvcc/backend/backend.go:161 +0x267
```

## Common Causes

1. Disk exhaustion caused corruption
2. An etcd bug

## Solution

You'll want to delete the entire etcd data directory. This directory is under `/Library/Application\ Support/Veertu/Anka/anka-controller` for the native macOS package, or in the location you specify in your docker configuration.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
