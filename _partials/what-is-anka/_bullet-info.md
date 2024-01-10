

* Easy to install and use; Docker-like experience.
* Built on Apple's native hypervisor and virtualization, utilizing macOS resource scheduling, power management, and consistency.
* Optimized and configurable VM networking.
* Optimized disk performance and optimizations to minimize host level usage.
* Access to Paravirtualized Graphics (GPU) inside of VMs.
* VM management through the Anka Virtualization CLI, UI app, or web based Build Cloud Controller UI and API.
* Ability to suspended and then instantly start VMs from a specific state.
* Attach physical USB devices (like an iPhone) to VMs for on-device testing.
* Compatibility with T2 enabled Apple hardware and all Silicon.
* Can be installed on any Apple supported macOS versions.
* Install and run any modern and Apple supported macOS version inside of VMs.

DevOps teams implementing Anka can expect flexibility, allowing them to install onto existing infrastructure and automation -- whether it's cloud providers like AWS or on-premises! The Anka Virtualization package can be installed onto any macOS hosts. The Anka Build Cloud Controller & Registry can be ran as standalone binaries, [a docker-compose package]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) and also with our [Build Cloud Controller & Registry Helm Chart](https://github.com/veertuinc/helm-charts/tree/main/charts/anka-build-cloud) for Kubernetes users.

You can find several [CI/CD plugins or integrations]({{< relref "plugins-and-integrations/_index.md" >}}) we maintain for our users that abstract the lower level work of pulling and starting VMs for your jobs. Whether it's on-demand/ephemeral, long-running, and single-use macOS VMs for your developers, iOS, or native app building/testing/CI/CD, Anka will be a good fit for you.

Anka also enables a docker-like experience for teams to create and store project specific VM templates (a.k.a "images") and tags, including commands to interact with the VM like start, stop, clone, suspend, modify the VM configuration (like cpu or ram), and execution of VM level shell commands.

* Use the [Anka Virtualization CLI]({{< relref "anka-virtualization-cli/getting-started" >}}) and [Packer](https://github.com/veertuinc/packer-builder-veertu-anka) to create Anka VM Templates for different versions of macOS, Xcode, etc.
* Store your VM Templates with a specific Tag in the Anka Build Cloud Registry so you can distribute or pull them to different machines and ensure the same starting VM state for every single CI/CD job that runs a VM. You can clone all VMs from a single base VM and they will re-use layers, optimizing disk space on both the Registry and Anka Nodes/hosts.

![Anka Registry pull and show VM]({{< siteurl >}}images/what-is-anka/registry-list-pull-show.png)
