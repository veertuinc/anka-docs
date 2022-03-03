---
title: "Anka VMs are in a \"failed\" status"
linkTitle: "Anka VMs are in a \"failed\" status"
weight: 2
---

## Scenario

Anka show indicates a `"failed"` status:

```
usr@veertus-Mac-mini-5 Anka % sudo anka list
+-------------------------------------------------+--------------------------------------+---------------------+---------+
| name                                            | uuid                                 | creation_date       | status  |
+-------------------------------------------------+--------------------------------------+---------------------+---------+
| m1-beta-vanilla (t1)                            | 2fe05256-8898-4e55-a3c5-5a4b0f20e66e | Jan 3 22:13:43 2022 | stopped |
+-------------------------------------------------+--------------------------------------+---------------------+---------+
| mgmtManaged-m1-beta-vanilla-1643843806808744000 | 3f8d5a15-2625-404f-b2e0-56df3b8e4c10 | Jan 3 22:13:43 2022 | running |
+-------------------------------------------------+--------------------------------------+---------------------+---------+
| mgmtManaged-m1-beta-vanilla-1643843808109609000 | 455ebee6-9ec3-4d31-93a2-ba4339ea6ada | Jan 3 22:13:43 2022 | running |
+-------------------------------------------------+--------------------------------------+---------------------+---------+
| mgmtManaged-m1-beta-vanilla-1643844420128700000 | eeb96388-1787-41e0-9dae-a6e252dab798 | Jan 3 22:13:43 2022 | failed  |
+-------------------------------------------------+--------------------------------------+---------------------+---------+
```

## Common Causes & Solutions

1. [No active/logged in (to UI) user, Autologin disabled, and a few other prep steps aren't performed.]({{< archRelRef "Anka Build Cloud/preparing-and-joining-your-nodes.md" >}})
2. Host or VM resources were exhausted. We recommend checking host vCPUs, RAM, total VMs running on the host, and then how many resources each VM can run and be sure to tweak the VM configs to prevent overuse.

{{< hint warning >}}
VMs in a failed state cannot be started until you `anka stop --force`. We also recommend `anka registry pull -l {VM Template Name}` to reset state.
{{< /hint >}}

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

