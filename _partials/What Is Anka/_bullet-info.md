---
---

* Easy to install packages.
* Built on the native Apple hypervisor, utilizing macOS resource scheduling, power management, and flexibility.
* Optimized VM network and disk performance using para-virtual drivers.
* VM management through the Anka Virtualization CLI, UI app, or web based Build Cloud Controller UI.
* Ability to suspended and then instantly start VMs from a specific state.
* Nested Virtualization for running Docker, Android Emulators, and others inside of the VM (intel only).
* Attach physical USB devices (like an iPhone) to VMs for on-device testing.
* Compatibility with T2 enabled Apple hardware.
* Can be installed on any Apple supported macOS versions.
* Anka VMs can install and run any modern and Apple supported macOS version.

DevOps workflows typically include lots of automation. Because of this, Anka has multiple options for you to either integrate with existing workflows/tools or create them from scratch:

* Easily create VM Templates (a.k.a "images") for different versions of macOS with the Anka CLI.
* Execute your project's dependency installation commands and scripts inside of VMs from the host terminal with [`anka run`](https://docs.veertu.com/anka/apple/command-line-reference/#run) and also directly in the VM with [`anka cp`](https://docs.veertu.com/anka/apple/command-line-reference/#cp) + [`anka run`](https://docs.veertu.com/anka/apple/command-line-reference/#run). Any manual steps you need to perform in the GUI can be done through VNC or automated with [anka click scripts](https://github.com/veertuinc/anka-click-scripts).
* Write scripts for automated Template creation in your preferred language, or use our [packer builder](https://github.com/veertuinc/packer-builder-veertu-anka).
* Store your VM Templates with a specific Tag in the Anka Cloud Registry so you can distribute or pull them to different machines and ensure the same VM state for every single CI/CD job. You can clone all VMs from a single base VM and they will re-use layers, optimizing disk space on both the Registry and Anka Nodes/hosts.
* Manage your VM Instances from the Anka Build Cloud Controller's UI and/or REST API.
* Get up and running starting jobs quickly with [one of our maintained CI plugins or integrations]({{< relref "Plugins and Integrations/_index.md" >}}).
