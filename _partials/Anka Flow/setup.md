---
date: 2019-07-03T22:24:47-05:00
title: "Setup"
linkTitle: "Setup"
weight: 2
description: >
  Setting up Anka Flow, How to install, uninstall and upgrade Anka Flow.
---
## Installation and Configuration

Anka when activated with the Anka Flow license enables installation of Anka package on the developer mac machines. It enables the developers to create and work inside Anka macOS VMs on their local machines. Developers can use the [`anka run`]({{< relref "arm/command-line-reference.md#run" >}}) interface to work inside Anka VMs in a container like manner.  

You can also use Anka Flow in combination with Anka Build and enable your developers to pull the build environments from the central registry on their local machines.

### Install

Install Anka package on the mac developer machine. 

> Anka Flow is only supported on MacBook models and iMac.  

1. Follow the [official installation guide]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}})

2. Activate with your Anka Flow license key.  
  ```bash
  anka license activate <key>
  ```