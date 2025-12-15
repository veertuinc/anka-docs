---
title: "license expiration is in X days, preventing non-interactive activation"
weight: 1
---

## Scenario

Anka license activate throws `license expiration is in X days, preventing non-interactive activation` when attempting to use `--force`

## Common Causes

1. To prevent abuse of the license activation process, the license server will prevent non-interactive activation if the license is greater than 14 days from expiration.

## Solution

Fully remove the license and activate it again.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

