---
title: "Missing gitlab-runner. Uploading artifacts is disabled"
weight: 3
---


## Scenario

The gitlab runner and our custom executor is working fine, but you cannot upload artifacts. You see `Missing gitlab-runner. Uploading artifacts is disabled` in the logs.

## Solution

You need to add the proper `PATH=` to your gitlab-runner binary. This can be done inside of the config under `[[runners.environment]]`.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

