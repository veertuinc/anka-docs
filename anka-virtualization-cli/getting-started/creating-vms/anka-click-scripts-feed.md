---
title: "Anka Click Scripts feed"
linkTitle: "Click Scripts feed"
weight: 3
description: >
  Update anka create automation scripts without upgrading the Anka CLI
aliases:
  - "/anka-virtualization-cli/getting-started/anka-click-scripts-feed/"
---

Starting in Anka 3.7.0, [Anka Click Scripts](https://github.com/veertuinc/anka-click-scripts) used during `anka create` download from a remote feed. You can get new scripts and macOS support without installing a new Anka package.

## Feeds

| Feed | URL |
|------|-----|
| Production (default) | `https://downloads.veertu.com/click-scripts/v1/feed.json` |
| Edge (pre-release) | `https://downloads.veertu.com/edge-click-scripts/v1/feed.json` |

Switch feeds:

```bash
❯ anka config feed_url https://downloads.veertu.com/edge-click-scripts/v1/feed.json
```

Use only scripts bundled with your installed Anka version:

```bash
❯ anka config feed_url ""
```

## Troubleshooting

If creation fails after a feed update, clear cached scripts and retry:

```bash
rm -f ~/.anka/tools/*
anka create ...
```

See [VM creation is stuck or failing]({{< relref "anka-virtualization-cli/troubleshooting/cli/anka-create-stuck-or-failing.md" >}}).
