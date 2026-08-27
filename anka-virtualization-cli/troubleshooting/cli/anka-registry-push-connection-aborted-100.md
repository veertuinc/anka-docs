---
title: "anka registry push \"-anka: connection aborted\" after 100%"
linkTitle: "anka registry push \"-anka: connection aborted\" after 100%"
weight: 3
---

## Scenario

Registry push reaches 100% and then shows `connection aborted`:

```bash
❯ anka registry push myVM -t v1
Uploading files  [####################################]  100%
-anka: connection aborted
```

## Common Causes & Solutions

1. You're running an NGINX proxy and `proxy_request_buffering` is enabled.
2. You're using a load-balancer or other type of middleman for the networking between Anka CLI/host and Registry which has a timeout value which is being reached.
3. Your network does not support the size of files being transferred. Have your networking team determine the cause and configuration changes necessary.
4. Slow or unstable uploads on high-latency links. Starting in Anka 3.8.0, try increasing `anka config send_buffer_size` and `anka config recv_buffer_size` (default `0` uses curl defaults). See [Tuning registry push performance]({{< relref "anka-build-cloud/working-with-registry-and-API.md#tuning-registry-push-performance" >}}).

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

