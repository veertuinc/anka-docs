---
title: "Firewall rules not blocking controller to registry traffic"
linkTitle: "Firewall rules not blocking controller to registry traffic"
weight: 1
---

## Scenario

You use a cloud provider like GCP and have set up firewall rules to block all traffic. You then create a firewall rule allowing traffic from your Anka Build Cloud Controller to your Registry.

Someone then on your team disables the firewall rule allowing traffic from your Anka Build Cloud Controller to your Registry. However, the Controller looks as if it's still working and able to talk to the Registry.

## Solutions

Certain cloud providers only apply firewall rules to new connections, not ones that exist at the time of the firewall rule being created/enabled. In the Controller, we use code that keeps existing connections open so they can be reused. This means that the firewall rule will not apply to existing connections, but will apply to new connections. To prevent this, you can set `ANKA_MAX_IDLE_CONNECTION_PER_HOST` to `-1` in your Controller and it will not keep existing connections open. This can impact performance, but will also immediately show you if there is a communication problem between the Controller and Registry.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
