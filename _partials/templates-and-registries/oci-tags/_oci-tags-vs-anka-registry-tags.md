Full tags and thin tags behave differently on a VM. The type of tag sets the behavior. The registry does not.

- **A full tag includes the macOS disk.** The Anka Registry stores only full tags. An OCI registry stores a full tag when you push with `push_mode` `1` and without `--thin`. A pull of a full tag puts the VM in the exact state of that tag. This occurs even if you changed the VM and did not push a new tag. The pull removes the thin tags that you pulled before it.
- **A thin tag holds only file layers.** It does not include the macOS disk. Only an OCI registry stores thin tags. A pull of a thin tag adds its files to the VM. It does not remove files from the VM, unless the tag has a [whiteout]({{< relref "templates-and-registries/using-an-oci-registry/appending-archives.md#remove-a-file-with-a-whiteout" >}}). Thin tags stack on the VM in the order that you pull them.
- **A second pull of a thin tag shows `already up-to-date`.** Anka does not apply the same layers again.
- **Pull the full tag first, then the thin tags.** Pro tip: keep a full tag under your thin tags. When the thin tags put the VM in a bad state, pull that full tag again to get a clean VM.
