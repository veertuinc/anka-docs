---
date: 2019-07-03T22:24:47-05:00
title: "Setup"
linkTitle: "Setup"
weight: 2
description: >
  Anka Build Cloud setup instructions.
---
[//]: # (TODO: find a place for this (if any) )

## Introduction
Registry and Controller services are required to configure an Anka Build private macOS cloud. Registry and controller are packaged in two flavors.  

1. Linux (CentOS 7) Docker container package - anka-controller-registry-XXX.tar.gz
2. Mac Application package - AnkaControllerRegistry-XXX.pkg  

Download the package that you want to install from [Anka Build Download page](https://veertu.com/download-anka-build/)

Default setup installs controller and registry together. You can configure them to run on two separate Linux instances and/or mac.

## Ports Information
CI system calls the Anka Controller over http or https, default port is 80.

Anka Controller does not communicate with Anka Build nodes. Nodes communicate with controller through a Queue service(installed as part of controller).

The default port for Anka registry is 8089.
All the default port and other configuration settings can be modified.