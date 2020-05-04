---
date: 2019-12-12T00:00:00-00:00
title: "What is Anka?"
linkTitle: "What is Anka?"
weight: 1
description: >
  All about Veertu's Anka Software
---

Anka virtualizes macOS to create, run macOS VMs on top of macOS and enables you to use automated and container like DevOps workflows when working with macOS virtual machines and macOS virtual machines with connected physical iOS devices. Anka exposes the following features:

## General
* Easy installation
* Native hypervisor - uses macOS resource scheduling and power management for guest VMs
* Instant boot for macOS VMs
* Para virtual drivers increase network and disk performance inside Anka macOS VMs
* `anka` CLI - automate iOS CI management workflows, similar to working with container
* Supports running docker and Android Emulator inside macOS VMs
* Works with T2 enabled Mac hardware

## Continuous Delivery and Integration
* Automate the provisioning of on-demand virtual machines
* Integrate with your CI system using `Controller` REST APIs for VM management
* Manage macOS infrastructure-as-a-code
* _INSTANT BOOT_ - Eliminates the long boot time issue with macOS VMs, making running multiple VMs extremely fast and responsive
* Use `Registry` to version and tag virtual machines for distribution
* Easily connect/remove physical iOS devices to your VMs for on-device testing

## What makes Anka different?

Unlike existing macOS virtualization solutions, Anka lets the macOS operationg system manage virtual machines in the same manner as other native macOS applications. Anka virtualization uses macOS native Hypervisor features. As a result, it can run on any mac heardware, including the ones with T2 security chip.

Combined with the powerful `anka` CLI, Anka makes creating and running macOS VMs extremely automated, fast and reliable. Using `anka`, `Controller` & `Registry` you can execute simple workflows to quickly build macOS virtual machines and deploy them to your developers, or CI with ease.

`anka` allows for other advantages as well, such as _INSTANT START_, to make multiple virtual machines extremely fast and responsive to your needs. Anka Registry lets you tag and version control your VMs, and is built specifically with DevOps workflows in mind. Lastly, Anka is the **ONLY** virtualization solution geared specifically towards making macOS and iOS CI workflows easy to manage. `anka` allows you to use [_real_ iOS devices](../testing-on-devices) for testing workflows while connected to your Apple hardware. Anka provides commands to easily manage the devices, check them out, or move them between test environments.

## Understanding VM Templates, Tags, and Disk Usage

{{< include file="./shared/content/en/docs/What Is Anka/partials/understanding-templates-and-tags.md" >}}

--- 

### What's included in Anka Build?
Anka Build solution includes `anka.pkg`, `Controller/Registry` and plugins to integrate with existing commonly used CI systems like Jenkins, Teamcity, GitLab CI, BuildKite etc.

### What's included in Anka Flow?
Anka Flow solution includes `anka.pkg` to run on developer mac workstations. Doesn't require Anka Build. Can co-exit with Anka Build. Anka Flow runs on developer mac workstations (macbook models and iMac).

### What's included in Anka Secure?
Anka Secure solution includes `anka.pkg` to run on mac end clients and the `Registry`. Doesn't require Anka Build. Can co-exit with Anka Build. Anka Flow runs on developer mac workstations (macbook models and iMac).

### Who should use Anka Build?
Anka Build is for anyone who wants to configure a private macOS cloud for CI purposes. The private macOS cloud can be configured on-prem or on hosted macs.

### Who should use Anka Flow?
Anka Flow is for anyone wanting to maximize developer productivity by enabling iOS and macOS developers to build and test in local macOS VMs, pulled from the central Anka Build Registry.

### Who should use Anka Secure?
Anyone who is looking to provide sandbox and policy managed macOS environments to end users using mac hardware for privileged user data access, example technical support or for iOS developers and testers.
