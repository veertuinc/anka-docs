---
title: "Failed saving group + failed getting Registry permissions list: 404 page not found"
weight: 1
---

## Scenario

Inside of the Controller UI's Permissions page, saving a group throws `Failed saving group` and the Controller logs show `failed getting Registry permissions list: 404 page not found`.

## Solutions

Set `ANKA_LOCAL_ANKA_REGISTRY` in your controller config to the non-8085 port.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
