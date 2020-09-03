---
date: 2019-12-12T00:00:00-00:00
title: "What is Anka?"
linkTitle: "What is Anka?"
weight: 1
description: >
  All about Veertu's Anka Software
---

Anka is a suite of software for creating and managing macOS VMs to run on top of Apple hardware and macOS. It's built on top of the native [Apple Hypervisor](https://developer.apple.com/documentation/hypervisor) technology for maximum performance. It enables the creation of or integration with existing container based DevOps workflows. Unlike existing macOS virtualization solutions, Anka lets the macOS operating system manage virtual machines in the same manner as other native macOS applications.

> Anka can be installed on macOS machines running High Sierra (10.13.x) or higher.

* Easy to install
* Built on the native Apple hypervisor, utilizing macOS resource scheduling, power management, and flexibility
* Optimized VM network and disk performance using para-virtual drivers
* VM management through the Anka Virtualization CLI, UI app, or web based Build Cloud Controller UI
* Ability to suspended and then instantly start VMs from a specific state
* Nested Virtualization for running Docker containers inside of your Anka VMs
* Attach physical USB devices (like an iPhone) to VMs for on-device testing
* Compatibility with T2 enabled Apple hardware

DevOps workflows typically include lots of automation. Because of this, Anka has multiple options for you to either integrate with existing workflows/tools or create them from scratch:

* Build VM Templates (a.k.a "images") for different versions of macOS
* Create Tags on top of a VM Template containing the different dependencies for each of your projects
* Write scripts for automated Template & Tag creation in your preferred language, or use our [packer builder](https://github.com/veertuinc/packer-builder-veertu-anka)
* Store your VM Templates and Tags in the Anka Cloud Registry so you can distribute or pull them to different machines and ensure the same VM state
* Manage your VMs from the Anka Build Cloud Controller's UI or REST API
* Get up and running quickly with [one of our maintained CI plugins or integrations]({{< relref "docs/CI Plugins and Integrations/_index.md" >}})

> Information about how VM Template and Tags work can be found in our [Getting Started > Creating your first VM]({{< relref "docs/Getting Started/creating-your-first-vm.md" >}}) guide.

---

## Anka Develop

### What's included in Anka Develop?
Developers with laptops (Macbook, Macbook Pro, and Macbook Air) can use the free Anka Develop license to create and start a single VM for local development and testing. Users can expect all features except for suspending, tagging, and registry commands.

### Who should use Anka Develop?
Anka Develop is for developers wanting to test their applications in a sandbox environment.

---

## Anka Build

### What's included in Anka Build?
The Anka Build solution includes the CLI, Cloud Controller (GUI + API) & Registry, and plugins to integrate with commonly used CI systems like Jenkins, Teamcity, GitLab, Buildkite and GitHub Action.

### Who should use Anka Build?
Anka Build is for anyone wanting to configure a private macOS cloud in their CI system. The private macOS cloud can be configured using on-prem or offsite hardware.

---

## Anka Flow

### What's included in Anka Flow?
The Anka Flow solution includes the CLI and Cloud Registry. Developers can use this to create Templates and Tags to run VMs locally on their workstations. It has no requirement on the Cloud Controller. Only available on Macbook and iMac hardware.

### Who should use Anka Flow?
Anka Flow is for anyone wanting to maximize developer productivity by enabling the running of builds and tests inside of VMs on their local machine. Each VM can contain specific dependencies and configurations for the project and avoid dependency problems/complexity on their local machine.

---

## Anka Secure

### What's included in Anka Secure?
The Anka Secure solution includes the CLI (with additional features like policy management) and Cloud Registry. Only available on Macbook and iMac hardware.

### Who should use Anka Secure?
Anka Secure is for users who are looking to provide sandbox and policy (network policies, sharing policies, etc) managed macOS environments to end users using Apple hardware. This is useful for technical support teams and remote developers with access to sensitive information and no home network security.
