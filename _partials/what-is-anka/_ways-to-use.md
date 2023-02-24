---
---

### Anka Virtualization CLI

There are many ways in which our customers utilize the Anka Virtualization CLI to automate the VM creation and preparation process:

- Through the `anka create` command on the host machine:
    ```
    ❯ anka create -a 13.1 13.1-arm
    ```
- By executing your project installation commands and scripts inside of VMs from the host terminal with [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}) and also directly inside the VM with [`anka cp`]({{< relref "anka-virtualization-cli/command-line-reference.md#cp" >}}) & [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}).
- Any manual steps you need to perform in the GUI can be done through VNC or automated with [Anka Click Scripts](https://github.com/veertuinc/anka-click-scripts).
- Create Packer Templates and run them to perform the steps for VM creation and preparation with our [packer builders and post-provisioner](https://github.com/veertuinc/packer-builder-veertu-anka)..

{{< rawhtml >}}<br />{{< /rawhtml >}}

---

{{< rawhtml >}}<center>{{< /rawhtml >}}

##### [Check out the Getting Started installing and using Anka CLI Guide.]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}})


{{< rawhtml >}}</center>{{< /rawhtml >}}

---

![Anka list start and run]({{< siteurl >}}images/what-is-anka/anka-list-start-run.png)
