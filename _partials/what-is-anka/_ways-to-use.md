

### Anka Virtualization CLI

There are many ways in which our customers utilize the Anka Virtualization CLI to automate the VM creation and preparation process:

- Through the `anka create` command on the host machine:
    ```
    ❯ anka create my-ci-vm 26.6.2
    ```
- By executing your project installation commands and scripts inside of VMs from the host terminal with [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}) and also directly inside the VM with [`anka cp`]({{< relref "anka-virtualization-cli/command-line-reference.md#cp" >}}) & [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}).
- Any manual steps you need to perform in the GUI can be done through VNC or automated with [Anka Click Scripts](https://github.com/veertuinc/anka-click-scripts).
- By pulling tools onto the VM as [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}), for example a Homebrew bottle from `ghcr.io`, without installing them inside the VM:
    ```
    ❯ anka pull my-ci-vm oci://ghcr.io/homebrew/core/cmake:4.3.4 --transform 's|cmake/4.3.4|usr/local'
    ```
- By listing the macOS version and the OCI tags in an [Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}), and creating the VM with `anka create my-ci-vm ./Ankafile`.
- Create Packer Templates and run them to perform the steps for VM creation and preparation with our [Packer plugin](https://github.com/veertuinc/packer-plugin-veertu-anka).

{{< rawhtml >}}<br />{{< /rawhtml >}}

---

{{< rawhtml >}}<center>{{< /rawhtml >}}

##### [Check out the Getting Started installing and using Anka CLI Guide.]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}})


{{< rawhtml >}}</center>{{< /rawhtml >}}

---

![Anka list start and run]({{< siteurl >}}images/what-is-anka/anka-list-start-run.png)
