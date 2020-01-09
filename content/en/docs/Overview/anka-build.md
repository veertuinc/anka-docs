
---
date: 2019-12-12T00:00:00-00:00
title: "Anka Build"
linkTitle: "Anka Build"
weight: 4
description: >
  Anka Build solution is used configure a private macOS cloud on which you can create, manage and distribute macOS reproducible virtual environments for iOS and macOS CI
---

###  What is Anka Build?
Anka Build solution is used configure a private macOS cloud on which you can create, manage and distribute macOS reproducible virtual environments for iOS and macOS CI.

The core of the Anka toolset is a native macOS hypervisor built on top of FreeBSD `bhyve` and `xhyve` and leverages Apple's macOS `Hypervisor.Framework` for virtualization. Anka hypervisor includes special PV network and disk drivers that are required for high performance operations inside the Anka VMs. This is available as Anka binery package. The Anka toolset also includes Cloud management service - `Controller` and a `Registry`. Anka `Controller` is used to configure and manage macOS cloud on a cluster of mac hardware host machines running Anka Build package, for CI purposes. Anka `Registry` is used to version control macOS VMs and distribute them to Anka Build private macOS cloud.

The Anka virtualization application package comes with a command line interface (`anka`) that allows for easy, straightforward management of guest virtual machines built with Anka.

As you'll learn, `anka` also provides the ability to interact with a remote Anka registry, and is purpose-built for engineers working in macOS or iOS development workflows. We think of Anka as a simple toolset that allows you to build your own private on-prem or hosted macOS private cloud and manage the macOS Infrastructure-as-a-code, whatever your purpose may be.

Anka interface allows devops to manage macOS build and test infrastructure as a code, very similar to working with containers.



