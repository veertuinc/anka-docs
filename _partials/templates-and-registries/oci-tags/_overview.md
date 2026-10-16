Anka is now compatible with OCI layers. They are related to Anka Template Tags, so we'll call them OCI Tags for the rest of this guide.

As expected with OCI layers with other tools, an OCI tag from one VM can be pulled onto any other VM. The other VM can have a different name, a different macOS version, or live on another host. Keep the vanilla macOS disk as a full tag in the Anka Registry or in an OCI registry, and pull that tag when a host needs the foundation. Store each tool as its own OCI tag in an OCI registry, and pull those tags when the VM needs them.

{{< hint warning >}}
**Store OCI tags in an OCI registry.** Use an `oci://` URL or an `https://` URL whose path contains `/v2/`. The Anka Registry does not store these types of tags. A push to the Anka Registry ignores `push_mode` and `--thin`, and always stores the full tag.
{{< /hint >}}

{{< hint info >}}
OCI layers compression can be controlled with the `ANKA_COMPRESSION_LEVEL` environment variable. Setting it to `1` turns on gzip and sets zlib’s level to 1 (fastest / lightest compression).
{{< /hint >}}
