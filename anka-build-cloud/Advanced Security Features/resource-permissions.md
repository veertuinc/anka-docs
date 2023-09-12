---
date: 2019-12-12T00:00:00-00:00
title: "Resource Permissions for Permission Groups"
weight: 5
description: >
  How to protect your Controller UI, API, and Registry API with granular permissions for Resources like Nodes and Templates.
---

{{< include file="_partials/anka-build-cloud/advanced-security-features/resource-permissions.md" >}}

##### Answers to Frequently Asked Questions

- The Nodes joined to the Controller must have Permissions to access the Template being used to start a VM.
- Node Groups differ from Permissions Groups and are disabled when this feature is enabled.
- Save Image requests can only target Instances that belong to a group the user has access to.

