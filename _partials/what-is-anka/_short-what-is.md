Veertu's Anka and Build Cloud is a suite of software tools built for small or enterprise level macOS virtualization needs. It enables the creation, storage, and management of macOS virtual machines (VMs), offering a user experience similar to what Docker and Docker Hub provide for container management, but with an added orchestrator component. Anka is specifically designed for DevOps, CI/CD, and Automation teams that need to build and test macOS or iOS applications.

Anka is a mature and battle-tested solution for small or large-scale macOS virtualization. Leveraging a combination of custom and Apple hypervisor/virtualization technology, Anka offers enhanced performance and security. It provides a simple CLI for generating Anka macOS VMs as well as tools like [Packer](https://github.com/veertuinc/packer-plugin-veertu-anka). You can then manage the VMs as Templates/Tags in the Anka Registry (part of the Anka Build Cloud), or in an OCI registry that you already use, such as Docker Hub, Amazon ECR, or Google Artifact Registry. Push and pull them to and from hosts in your macOS cluster. Starting in Anka 3.10.0, you can also store each tool, such as Xcode, as its own thin [OCI tag]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}). Install a tool once, then add it to any VM, and update it without a rebuild of the full VM.

{{< hint info >}}
Homebrew packages are also thin tags, so you can add them to a VM without Homebrew installed in the VM.
{{< /hint >}}

Alongside of the Anka Virtualization CLI is the [Anka Build Cloud]({{< relref "../../anka-build-cloud/_index.md" >}}) which serves as a unified management interface for Anka Build Nodes, VM Templates/Tags, VM instances, and logs. It features intelligent queueing and load balancing for handling numerous concurrent CI/CD job macOS VM requests. Additionally, it [integrates with widely-used automation platforms and tools]({{< relref "plugins-and-integrations/_index.md" >}}) such as [Jenkins]({{< relref "plugins-and-integrations/controller-+-registry/jenkins.md" >}}), [Github Actions]({{< relref "plugins-and-integrations/controller-+-registry/github-actions.md" >}}), [Buildkite]({{< relref "plugins-and-integrations/controller-less/buildkite.md" >}}), [TeamCity]({{< relref "plugins-and-integrations/controller-+-registry/teamcity.md" >}}), [GitLab]({{< relref "plugins-and-integrations/controller-+-registry/gitlab-custom-executor.md" >}}), and provides an [API for custom integrations]({{< relref "anka-build-cloud/working-with-controller-and-api.md" >}}).

Anka is a robust and adaptable macOS virtualization platform that can enhance your DevOps and iOS CI/CD/Automation. It is straightforward to use and customize, and offers a broad array of features and capabilities.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Anka High Level]({{< siteurl >}}images/what-is-anka/what-is-anka.webp)
{{< rawhtml >}}</center>{{< /rawhtml >}}
