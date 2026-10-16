Anka is now comptible with OCI layers. They fall under Anka Template Tags, so we'll call them OCI Tags for the rest of this guide.

As expected with OCI layers with other tools, an OCI tag from one VM can be pulled onto any other VM. The other VM can have a different name, a different macOS version, or live on another host. Keep the vanilla macOS disk as a block tag in the Anka Registry, and pull that tag when a host needs the foundation. Store each tool as its own OCI tag in an OCI registry, and pull those tags when the VM needs them.

{{< hint warning >}}
**Store OCI tags in an OCI registry.** Use an `oci://` URL or an `https://` URL whose path contains `/v2/`. The Anka Registry does not store these types of tags.
{{< /hint >}}

{{< hint info >}}
OCI layers compression can be controlled with the `ANKA_COMPRESSION_LEVEL` environment variable. Setting it to `1` turns on gzip and sets zlib’s level to 1 (fastest / lightest compression).
{{< /hint >}}

OCI tags and Anka Registry tags behave differently. Here are some important differences to be aware of:

- OCI tags are stored only in an OCI registry.
- Pulling an OCI tag that is already on the VM shows `already up-to-date`. It does not remove contents from the VM. A pull of a new Anka Registry tag will remove the other OCI tags you pulled and revert back to its original state. Whereas with Anka Registry tags, on pull, you'll get the exact state of the VM when the tag was created, always, even if you made changes but haven't pushed a new tag.
- OCI tags stack on a VM. They do not remove contents from the VM (unless you [whiteout]({{< relref "whats-new/anka-3.10.0/index.md#remove-a-file-with-a-whiteout" >}}) a file in the archive you pull). Pull an OCI tag onto a VM that already has an Anka Registry tag on it, and you can revert to it whenever you mess up the OCI tags and want a clean slate.
- Anka Registry tags and OCI tags can be combined only if you first pull the Anka Registry tag.
