---
title: "ETCD cluster won't start and fails with \"request sent was ignored by remote peer due to cluster ID mismatch\""
weight: 1
---

## Scenario

Starting your etcd cluster throws an error like:

```json
{"level":"warn","ts":"2022-03-09T01:06:57.767Z","caller":"rafthttp/stream.go:653","msg":"request sent was ignored by remote peer due to cluster ID mismatch","remote-peer-id":"99fbb86961c11d8f","remote-peer-cluster-id":"d0d0a4fb77ca1d5f","local-member-id":"5892b00848500dd8","local-member-cluster-id":"464487e7a78e118c","error":"cluster ID mismatch"}
```

## Common Causes

1. Kubernetes PVC/PV contain older configuration or cluster information and you're trying to start with `initialClusterState: new`.

## Solution

1. This is a common problem with Bitnami's Helm Chart. Be sure you follow the proper instructions when adding new replicas or upgrading to a newer version of the Helm chart as `helm install` and `helm upgrade` behave differently. Otherwise, you need to wipe your PVC/PV to successfully start a new etcd cluster. [MORE INFO](https://docs.bitnami.com/kubernetes/faq/troubleshooting/troubleshooting-helm-chart-issues/#persistence-volumes-pvs-retained-from-previous-releases)

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
