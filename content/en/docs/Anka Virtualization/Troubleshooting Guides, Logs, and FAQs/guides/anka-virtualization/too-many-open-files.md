---
title: "\"Too many open files\" in {UUID}.log"
linkTitle: "\"Too many open files\" in {UUID}.log"
weight: 3
---

## Scenario

VM fails to start or boot and the `/Library/Logs/Anka/{UUID}.log` file shows:

```
Failed to open 05e38e83d0c446d095613e184984b7e6.ank
Failed to open 05e38e83d0c446d095613e184984b7e6.ank
Failed to open 05e38e83d0c446d095613e184984b7e6.ank
cli2: error in accept: Too many open files
ankahv: boot failed
```

## Common reasons & Solutions

1. System launchctl maxfiles is too small: `sudo launchctl limit maxfiles 4096 unlimited`
2. `chunk_size` is too small; requires re-creating your templates/tags after changing the config

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

