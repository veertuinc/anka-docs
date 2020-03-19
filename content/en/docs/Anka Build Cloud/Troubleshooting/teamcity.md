---
title: "Teamcity doesn't start VMs"
linkTitle: "Teamcity doesn't start VMs"
weight: 4
draft: true
description: >
  How to debug Anka Teamcity plugin common issues.
---


## Scenario

You configured Anka Build Cloud alongside the Teamcity plugin. You are now trying to run a build but it doesn't start and you are not seeing any new agents connected to Teamcity.  

## Common reasons

1. Configuration mistakes
2. SSH port forwarding not set correctly on VM 
3. The Anka Cloud can't run VMs (see [VM is stuck at scheduling]({{< relref "vm-stuck-scheduling.md">}}))