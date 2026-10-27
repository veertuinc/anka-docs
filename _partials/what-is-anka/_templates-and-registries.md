### Storing and sharing VM templates

After you prepare a VM, you push it to a registry as a template. Your other hosts then pull it. Anka gives you two types of registry. You can use one of them, or both together:

- **The Anka Registry** is part of the Anka Build Cloud. It stores full VM templates, and the Build Cloud Controller pulls templates from it.
- **Your OCI registry** (Anka 3.9.0 and later) can be Docker Hub, Amazon ECR, Google Artifact Registry, JFrog Artifactory, Zot, or another OCI Distribution–compliant registry. You use it from the `anka` CLI. Starting in Anka 3.10.0, it can also store **thin tags**: one tool per tag, without the macOS disk, such as Xcode, that you install once and add to any VM.

{{< hint info >}}
Homebrew packages are also thin tags, so you can add them to a VM without Homebrew installed in the VM.
{{< /hint >}}

Already use the Anka Registry? You can [move your tools to thin tags]({{< relref "templates-and-registries/moving-to-oci-tags.md" >}}) one step at a time and keep your current templates.

{{< rawhtml >}}<center>{{< /rawhtml >}}

##### [Choose where to store your templates.]({{< relref "templates-and-registries/_index.md" >}})

{{< rawhtml >}}</center>{{< /rawhtml >}}
