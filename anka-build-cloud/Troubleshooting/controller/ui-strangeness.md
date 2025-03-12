---
title: "UI Slowness, Nodes and Instances temporarily showing Offline or missing, or other UI related issues"
weight: 1
---

## Scenarios

1. Loading the UI seems to be very slow
2. Node status all seem to be delayed or alternating between different states randomly
3. Instances don't show up immediately or at all

> Confirm an `average_queue_time` of > 5 when curling the controller's `api/v1/stats` endpoint

## Common Causes

1. Lots of VMS, Nodes, and not enough load balancing/instances of the controller, etcd, etc.
2. ETCD is very sensitive to disk latency; not using SSDs for the etcd storage
3. Session stickynesss is not enabled for your load balancer

## Solutions:

- Check your load balancer session stickyness settings and increase them to several minutes
- Increase the space quota from the default 2GB: [https://etcd.io/docs/current/op-guide/maintenance/#space-quota]
- Rejoin your nodes with a higher `--heartbeat` value (> 20s)
- Upgrade the host's disk to an SSD or faster disk
- Set `ANKA_NUM_WORKERS` to more than the default 2 in the Controller configuration.

> State changes can also be caused by network issues between the Node and the controller. Check the node's `/var/log/veertu/anka_agent.ERROR` log to confirm you're not seeing timeouts or connection errors.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)