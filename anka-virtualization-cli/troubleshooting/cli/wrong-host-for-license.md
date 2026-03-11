---
title: "Executing a command shows Wrong host for license (-4)"
weight: 1
---

## Scenarios

- Executing a command shows `Wrong host for license (-4)`

## Solution

{{< hint info >}}
Run the command with `anka --debug <command>` to see the full debug output.
{{< /hint >}}

Check `sudo RLM_DEBUG= RLM_DIAGNOSTICS= RLM_ACT_TIMEOUT=30000 anka --debug license validate` for any errors.

Typically this is caused by the CLI command triggering a security prompt on the host desktop/UI that cannot be seen from SSH. However, there are situations where buildkite agents are unable to see the license when they run commands as the normal non-root user on the host.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
