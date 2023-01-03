---
---

## Using Anka

There are many ways in which our customers utilize the Anka Virtualization software. The first is on its own as a CLI tool:

- Execute commands on the host machine, or wrap them in a script.
- Prepare your VMs with dependencies by executing your project installation commands and scripts inside of VMs from the host terminal with [`anka run`](https://docs.veertu.com/anka/apple/command-line-reference/#run) and also directly in the VM with [`anka cp`](https://docs.veertu.com/anka/apple/command-line-reference/#cp) & [`anka run`](https://docs.veertu.com/anka/apple/command-line-reference/#run).
  - Any manual steps you need to perform in the GUI can be done through VNC or automated with [anka click scripts](https://github.com/veertuinc/anka-click-scripts).
  - Create Packer Templates and run them with our [packer builder](https://github.com/veertuinc/packer-builder-veertu-anka).
- 

![Anka list start and run]({{< siteurl >}}images/what-is-anka/anka-list-start-run.png)


### Anka Build Cloud

Below are two of the most popular examples of how our customers set up Anka Virtualization with the Build Cloud.

{{< include file="_partials/What Is Anka/_controller-less.md" >}}

{{< include file="_partials/What Is Anka/_controller-and-registry.md" >}}
