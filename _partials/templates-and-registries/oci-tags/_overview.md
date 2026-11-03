Anka is compatible with the OCI Container Registry Protocol. A tag that you store in an OCI registry is an OCI tag. An OCI tag is one of two types:

- **A full tag** includes the macOS disk. It is the same as a tag in the Anka Registry. A VM can start from a full tag alone. Anka 3.9.0 and later can push and pull full tags with an OCI registry.
- **A thin tag** holds only file layers, such as the files of one tool. It does not include the macOS disk. Anka 3.10.0 and later can push and pull thin tags.

Marketing calls these thin tags **Portable Thin Tags**. In Anka they are thin tags: standard OCI filesystem layers (file-diff tarballs) without the macOS disk. “Thin” means less to store and less to move. “Portable” means you can pull the same tag onto compatible VMs. `--thin` on push takes only the last layer, not the full chain, and still produces a fully compliant OCI repository and tag. Third-party tools—malware and vulnerability scanners, software inventory, and OCI clients such as skopeo and oras—can process that layer. There is no vendor lock on the layer format.

As expected with the OCI Container Registry Protocol in other tools, you can pull a thin tag from one VM onto any other VM. The other VM can have a different name, a different macOS version, or live on another host. Keep the vanilla macOS disk as a full tag in the Anka Registry or in an OCI registry, and pull that tag when a host needs the foundation. Store each tool as its own thin tag in an OCI registry, and pull those tags when the VM needs them.

A tool update still means composing the thin-tag stack into each final template or tag: pull the thin tags onto the base, then push the finished tag. You do not need to start the VM to reinstall the tool.

You can also pull public OCI artifacts onto a VM. These artifacts are thin tags. Examples: Homebrew bottles from `ghcr.io`, GGUF models from Hugging Face (`oci://hf.co/...`), and other OCI-compliant layers such as Ollama image layers (`application/vnd.ollama.image`). See [Pull OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#pull-oci-tags" >}}).

{{< hint warning >}}
**Store thin tags in an OCI registry.** Use an `oci://` URL or an `https://` URL whose path contains `/v2/`. The Anka Registry does not store thin tags. A push to the Anka Registry ignores `push_mode` and `--thin`, and always stores a full tag.
{{< /hint >}}

{{< hint info >}}
Compression for the OCI Container Registry Protocol can be controlled with the `ANKA_COMPRESSION_LEVEL` environment variable. Setting it to `1` turns on gzip and sets zlib’s level to 1 (fastest / lightest compression).
{{< /hint >}}
