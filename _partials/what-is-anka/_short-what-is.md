---
---

Veertu's Anka is a suite of software tools built on the macOS virtualization platform. It enables the execution of single or multi-use macOS virtual machines (VMs) in a manner similar to Docker. Anka is specifically designed for DevOps and CI/CD teams that need to build and test macOS or iOS applications. Leveraging the official Apple hypervisor/virtualization technology, Anka offers enhanced performance and security.

Anka is also a user-friendly solution for large-scale macOS virtualization. It allows you to generate Anka macOS VMs using a simple CLI, infrastructure as code tools, manage VM tags with specific dependencies and states via the Anka Registry (part of the Anka Build Cloud), and execute Anka macOS VMs on any connected nodes.

Along side of the Anka CLI is the [Anka Build Cloud]({{< relref "../../anka-build-cloud/_index.md" >}}) which serves as a unified management interface for Anka Build Nodes, VM instances, VM Templates/Tags, and logs. It features intelligent queueing and load balancing for handling numerous concurrent CI/CD job macOS VM requests. Additionally, it integrates with widely-used CI/CD servers and tools such as Jenkins, Github Actions, Buildkite, TeamCity, GitLab, and provides an API for custom integrations.

Anka is a robust and adaptable macOS virtualization platform that can enhance your DevOps and iOS CI/CD processes. It is straightforward to use and customize, and offers a broad array of features and capabilities.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Anka High Level]({{< siteurl >}}images/what-is-anka/isoflow-build-cloud-v2.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}
