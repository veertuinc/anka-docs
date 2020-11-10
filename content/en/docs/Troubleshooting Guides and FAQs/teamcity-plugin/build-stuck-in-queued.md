---
title: "Builds stuck in queued"
linkTitle: "Builds stuck in queued"
weight: 4
draft: true
description: >
  How to debug teamcity not starting agents for a build.
---


## Scenario

You configured Anka Build Cloud alongside the Teamcity plugin. You are now trying to run a build but it doesn't start and you are not seeing any new agents connected to Teamcity.  

## Common reasons

1. Configuration mistakes
2. SSH port forwarding not set correctly on VM 
3. The Anka Cloud can't run VMs (see [VM is stuck at scheduling]({{< relref "docs/Troubleshooting Guides and FAQs/anka-build-cloud/controller/vm-stuck-scheduling.md">}}))