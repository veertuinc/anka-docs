---
title: "anka create is stuck or failing"
linkTitle: "anka create is stuck or failing"
weight: 1
---

## Scenarios

- The `anka create` runs for a while but gets stuck. Typically stuck at `prebooting` or `waiting for postinstall complete`.
- The Anka app creation process runs for hours and never completes.

## Common reasons and solutions

1. McAfee or other anti-virus is blocking networking initialization. McAfee will need to be disabled, or anka processes whitelisted.
2. The macOS installation has failed for some reason. You can open the `anka view {name}` to ensure that it is actually stuck or failed.
    - If so, try running `anka --debug create...` from your terminal to see if it works a second time. If it keeps failing, report the issue to Veertu support with the output from the `anka --debug create` command.
    - It's also possible that you haven't given enough resources to the VM being created. We recommend a minimum of 2 vcpus and 4GBs of memory for stability.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
