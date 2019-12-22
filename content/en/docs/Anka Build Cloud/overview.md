
---
date: 2019-12-12T00:00:00-00:00
title: "Overview"
linkTitle: "Overview"
weight: 1
description: >
  Overview of Anka Build Cloud.
---

## Software modules
Anka Build solution consists of several software modules.

### Anka Registry
Anka Registry provides an easy way to store, version, and distribute macOS VMs. 

### Anka Controller
Anka Controller provides a Rest API and a web UI for managing on-demand macOS VMs.

### Anka Agent
Anka Agent (included in `anka.pkg`) is a daemon that manages VMs on the mac hosts/nodes, on behalf of Anka Controller. 

[//]: # (TODO: find a better way to describe anka agent ^ )

### CI plugins 
The plugins are installed on your respective CI server (Jenkins, Teamcity etc).

## High level diagram

![High level architechture](/images/anka-build/high-level.png)
