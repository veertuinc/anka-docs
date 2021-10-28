---
---
* Easy to install.
* Built on the native Apple hypervisor, utilizing macOS resource scheduling, power management, and flexibility.
* Optimized VM network and disk performance using para-virtual drivers.
* VM management through the Anka Virtualization CLI, UI app, or web based Build Cloud Controller UI.
* Ability to suspended and then instantly start VMs from a specific state.
* Nested Virtualization for running Docker, Android Emulators, and others inside of the VM.
* Attach physical USB devices (like an iPhone) to VMs for on-device testing.
* Compatibility with T2 enabled Apple hardware.
* Can be installed on the majority of modern macOS versions (High Sierra 10.13.x or higher).
* Anka VMs can install and run any modern macOS version (High Sierra 10.13.x or higher).

DevOps workflows typically include lots of automation. Because of this, Anka has multiple options for you to either integrate with existing workflows/tools or create them from scratch:

* Build VM Templates (a.k.a "images") for different versions of macOS.
* Clone the VM Template and create new Templates containing the different dependencies for each of your projects.
* Write scripts for automated Template creation in your preferred language, or use our [packer builder](https://github.com/veertuinc/packer-builder-veertu-anka).
* Store your VM Templates with a specific Tag in the Anka Cloud Registry so you can distribute or pull them to different machines and ensure the same VM state.
* Manage your VMs from the Anka Build Cloud Controller's UI or REST API.
* Get up and running quickly with [one of our maintained CI plugins or integrations]({{< relref "intel/CI Plugins and Integrations/_index.md" >}}).

{{< hint info >}}
Information about how VM Template and Tags work can be found in our [Getting Started > Creating your first VM]({{< relref "intel/Getting Started/creating-your-first-vm.md" >}}) guide.
{{< /hint >}}