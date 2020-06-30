---
date: 2019-12-12T00:00:00-00:00
title: "License tiers"
linkTitle: "License Tiers"
weight: 12
description: >
  Anka Build Cloud license tiers.
---

## Anka Build Tier Datasheet

{{< include file="shared/content/en/docs/Licensing/partials/_data-sheet.md" >}}

### Enterprise Features
Anka Build Enterprise License tier enables additional features on top of Build Basic Tier.  

**Clustering(Grouping) - Enterprise and Enterprise Plus**
This feature enables users to group Anka Build nodes into clusters/groups. These clusters can then be specified selectively to provision VMs. When nodes are put in groups, the level of separation is provided at the controller level for VM provisioning requests initiated from the controller.

For example, you can create group A, group B. Then, as required some CI job requested VMs can only be provisioned on  Group A or Group B.

A fallback group can be defined for any group. The fallback group will pick up the VM provisioning request if the primary group is running at its capacity.

Managing group definitions - Can be done through controller Portal UI or Controller REST APIs.

Joining Anka Build nodes to the group - Can be done at the time of joining the node, through Controller REST APIs, Controller REST APIs.

Jenkins and TeamCity Anka Plugins expose grouping feature.

**Priority Scheduling - Enterprise and Enterprise Plus**
Priority parameter is exposed through â€œCreate VMâ€ Controller REST API and also through the controller portal UI. This parameter enables priority provisioning of the VM, when there are multiple requests in the controller queue.

Range - 1 - 1000. 1 - Indicates the most urgent. 1000 - Default for all

**USB support - Enterprise and Enterprise Plus**
Attach one or multiple devices (through the host USB interface) to dynamically provisioned VMs for testing. Exposed through controller REST APIs and command line interface.

**Basic controller authentication also includes REST APIs (Superuser login) (Available from Controller 1.1 release) - Enterprise and Enterprise Plus**

Authentication support includes Root token authentication access to the Controller Dashboard and certificate authentication for following clients - Build nodes, plugins, API access, anka command line access to the registry.


### Enterprise Plus Features
Anka Build Enterprise Plus License tier enables additional features on top of Build Basic and Enterprise Tiers. 

**Authentication and Authorization support for multi-users through SSO (Available from Controller 1.1 release) - Enterprise Plus**
Multi-user access authentication and authorized based access to Controller portal dashboard and REST API operations is provided through OpenID and LDAP SSO based integration.

**VM encryption of Build VMs at REST - Enterprise Plus**
Encrypt the build VM template at the time of VM creation, store it in the Anka Registry in an encrypted state. When this VM is used to build on the Anka Build nodes, it will be decrypted.

**VM Control through Policy - Enterprise Plus**
Manage the Build VMs access to local and remote resources through policies. This includes the ability to control VM access to host shared folders, USB, disk access, networking, external networking.

## Securing Anka Build Cloud

### Basic Tier
In the basic tier, you can use TLS to encrypt communication between Anka Build components.

**TLS based encryption** | **Details**
--- | ---
Anka Controller and Registry have built-in TLS support, configured through command-line arguments (or docker-compose.yml) | What is encrypted -  Communication between controller and dashboard, registry and dashboard, registry and Anka running on Build nodes/host mac machines
Plugins to the controller | What is encrypted - Communication between plugins and the controller REST APIs


### Enterprise Tier
Enterprise Tier supports authentication on top of TLS.

This tier supports 2 levels of authentication.

1. Root token authentication
2. Client Certificate authentication

This tier has no concept of authorization, so every authenticated connection is treated as if it has same permissions.

**Authentication Method** | **Details**
--- | ---
Root token authentication | Should be used to configure authenticated superuser access to the Anka Portal
Certificate authentication | Access from Anka host machines(for registry), Nodes(both registry and controller), Plugins, Controller and registry API access
