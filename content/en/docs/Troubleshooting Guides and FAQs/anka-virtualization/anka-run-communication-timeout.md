---
title: "license active: -anka: Could not get key information"
linkTitle: "license activate: -anka: Could not get key information"
weight: 1
---

## Scenario

Anka license activate throws `-anka: Could not get key information`

## Common reasons

1. Networking conditions prevent the license server from returning license information in the default timeout.

## Solution

Set the ENV `RLM_ACT_TIMEOUT=240` (or higher)

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

