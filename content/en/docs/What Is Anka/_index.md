---
date: 2019-12-12T00:00:00-00:00
title: "What is Anka?"
linkTitle: "What is Anka?"
weight: 1
description: >
  All about Veertu's Anka Software
---

Anka is a suite of software for creating and managing macOS VMs to run on top of Apple hardware and macOS. It allows the creation of or integration with existing container-like DevOps workflows.

* Easy installation
* Built on the native hypervisor, utilizing macOS resource scheduling and power management for Anka VMs
* Optimized VM network and disk performance using paravirtual drivers
* Manual or automated control of Anka VMs is available through our Anka CLI
* Anka VMs can be suspended and then started again instantly
* Run Docker and Android Emulator inside your VMs
* Easily attach physical iOS devices to your VMs for on-device testing
* Compatibility with T2 enabled Apple hardware

DevOps workflows typically include lots of automation. Because of this, Anka includes multiple techniques for you to either integrate with existing workflows/tools or create them from scratch.

* Build VM Templates (a.k.a "images") for different versions of macOS and then Tags containing various dependencies for your projects
* Store/Version Control your VM Templates and Tags in the Anka Cloud Registry so you can distribute them to different machines in a specific state
* A REST API for VM management
* Use [one of our maintained CI plugins]({{< relref "docs/Anka Build Cloud/CI Plugins/_index.md" >}})

## What makes Anka different?

Unlike existing macOS virtualization solutions, Anka lets the macOS operating system manage virtual machines in the same manner as other native macOS applications. Anka virtualization uses macOS native Hypervisor features, so run on any Apple hardware.

* With the Anka CLI, creating and running macOS VMs easy, fast, and reliable. 
* With the Anka Build Cloud Controller, you can manage your Nodes (host machines) and VMs through the GUI or API.
* With the Anka Cloud Registry you can keep track of VM Templates and versions (Tags) used by different projects within your organization. You can then distribute from the Anka Build Cloud Controller or manually pull them to any machine running the Anka CLI.

## Understanding VM Templates, Tags, and Disk Usage

{{< include file="./shared/content/en/docs/What Is Anka/partials/_understanding-templates-and-tags.md" >}}

---

## Anka Build

### What's included in Anka Build?
The Anka Build solution includes the CLI, Cloud Controller (GUI + API) & Registry, and plugins to integrate with commonly used CI systems like Jenkins, Teamcity, GitLab, and Buildkite.

### Who should use Anka Build?
Anka Build is for anyone wanting to configure a private macOS cloud in their CI system. The private macOS cloud can be configured using on-prem or offsite hardware.

---
---

## Anka Flow

### What's included in Anka Flow?
Anka Flow solution includes the CLI and Cloud Registry. Developers can use this to create Templates and Tags to run VMs locally on their workstations. It has no requirement on the Cloud Controller. Only available on Macbook and iMac hardware.

### Who should use Anka Flow?
Anka Flow is for anyone wanting to maximize developer productivity by enabling the running of builds and tests inside of VMs on their local machine. Each VM can contain specific dependencies and configurations for the project and avoid dependency problems/complexity on their local machine.

---
---

## Anka Secure

### What's included in Anka Secure?
Anka Secure solution includes the CLI (with additional features like policy management) and Cloud Registry. Only available on Macbook and iMac hardware.

### Who should use Anka Secure?
Anka Secure is for users who are looking to provide sandbox and policy (network policies, sharing policies, etc) managed macOS environments to end users using Apple hardware. This is useful for technical support teams and remote developers with access to sensitive information and no home network security.