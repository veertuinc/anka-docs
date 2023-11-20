---
title: "Anka Build Cloud 1.39.0"
weight: 4
---

{{< hint info >}}
Anka Build components support heterogeneous build nodes; You can connect both Anka 2 (intel processors) and Anka 3 (apple processors) nodes to your Build Cloud Controller/Registry.
{{< /hint >}}

## What is the Anka Build Cloud?

Docker and DockerHub revolutionized the way developers could build and test their software. However, Docker does not at the time of writing this support macOS. This is why we've created the Anka Build Cloud. The Anka Build Cloud is a suite of software which allows you to manage and store [Anka VM Templates and Tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#vm-clones" >}}) in a central repository, orchestrate on-demand (or persistent) macOS VMs for your CI/CD (or developers), and visualize usage or logs.

### Anka Controller

This is a web UI and REST API which helps manage VM Instances and [VM Templates and Tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#vm-clones" >}}).

### Anka Registry

This is a repository for your Anka [VM Templates and Tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#vm-clones" >}}).

### Anka Virtualization Nodes

These Nodes have our [Virtualization software]({{< relref "anka-virtualization-cli/_index.md" >}}) installed to run the VMs that you launch from the Controller (or through your CI/CD software). Once joined to the Controller, it will start an agent on the node to communicate and pull tasks from the controller queue.

---

![High level architecture]({{< siteurl >}}images/anka-build/high-level.png)

---

{{< hint info >}}
Several of our [CI/CD Plugins and Integrations]({{< relref "plugins-and-integrations/_index.md" >}}) require the Controller REST API.
{{< /hint >}}
