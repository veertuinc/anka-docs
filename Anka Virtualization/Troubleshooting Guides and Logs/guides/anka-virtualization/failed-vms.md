---
title: "Anka VMs are in a \"failed\" status"
linkTitle: "Anka VMs are in a \"failed\" status"
weight: 2
---

## Scenario

Anka show indicates `"failed"` for the status

## Common reasons & Solutions

1. [No active/logged in (to UI) user, Autologin disabled, and a few other prep steps aren't performed.]({{< ref "Anka Build Cloud/prepare-nodes.md" >}})
2. The failed state is somehow stuck. Perform `anka start {VM NAME}`, `anka stop -f {VM NAME}`, and then `anka registry pull {VM NAME}` to reset it.
## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

