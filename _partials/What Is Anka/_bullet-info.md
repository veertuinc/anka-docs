---
---

* Easy to install.
* Built on the native Apple hypervisor, utilizing macOS resource scheduling, power management, and flexibility.
* Optimized VM network and disk performance using para-virtual drivers.
* VM management through the Anka Virtualization CLI, UI app, or web based Build Cloud Controller UI.
* Ability to suspended and then instantly start VMs from a specific state.
* Nested Virtualization for running Docker, Android Emulators, and others inside of the VM (intel only).
* Attach physical USB devices (like an iPhone) to VMs for on-device testing.
* Compatibility with T2 enabled Apple hardware.
* Can be installed on any Apple supported macOS versions.
* Anka VMs can install and run any modern and Apple supported macOS version.

DevOps teams can expect flexibility, allowing them to plug into existing infrastructure and automation -- whether it's AWS or on-premises! We also provide several [CI/CD plugins or integrations]({{< relref "Plugins and Integrations/_index.md" >}}) to choose from. Whether it's on-demand/ephemeral, long-running, and single-use macOS VMs for your developer, iOS, or native app building/testing/CI/CD, Anka will be a good fit.

Anka also enables a docker-like experience for teams to create and store project specific VM templates and tags, including commands to interact with the VM like start, stop, clone, suspend, modify the VM configuration like cpu or ram, and execution of VM level shell commands.

* Easily create VM Templates (a.k.a "images") for different versions of macOS with the Anka CLI.
* Store your VM Templates with a specific Tag in the Anka Cloud Registry so you can distribute or pull them to different machines and ensure the same VM state for every single CI/CD job. You can clone all VMs from a single base VM and they will re-use layers, optimizing disk space on both the Registry and Anka Nodes/hosts.

![Anka Registry pull and show VM]({{< siteurl >}}images/what-is-anka/registry-pull-and-show.png)
