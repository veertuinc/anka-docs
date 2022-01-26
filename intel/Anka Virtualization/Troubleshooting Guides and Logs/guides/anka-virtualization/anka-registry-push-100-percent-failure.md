---
title: "Anka Registry Push shows connection aborted"
linkTitle: "Anka Registry Push shows connection aborted"
weight: 3
---

## Scenario

Registry push reaches 100% and then shows `connection aborted`:

```bash
❯ anka registry push myVM -t v1
Uploading files  [####################################]  100%
-anka: connection aborted
```

## Common reasons & Solutions

1. You're running an NGINX proxy and `proxy_request_buffering` is enabled.
2. You're using a load-balancer or other type of middleman for the networking between Anka CLI/host and Registry which has a timeout value which is being reached.
3. Your network does not support the size of files being transferred. You can set `sudo anka config chunk_size 536870912` and re-create your VM so that each of the files making up the VM never go past the `chunk_size` value.

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

