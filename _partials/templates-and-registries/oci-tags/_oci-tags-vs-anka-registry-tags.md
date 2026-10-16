OCI tags and Anka Registry tags behave differently. Here are some important differences to be aware of:

- OCI tags are stored only in an OCI registry.
- Pulling an OCI tag that is already on the VM shows `already up-to-date`. It does not remove contents from the VM. Pulling a Anka Registry Tag will force the exact state of the VM when the tag was created, always, even if you made changes but haven't pushed a new tag. OCI Tags pulled up until then will be removed. Ultimately, Anka Registry tags and OCI tags can be combined only if you first pull the Anka Registry tag.
- OCI tags stack on a VM. They do not remove contents from the VM (unless you [whiteout]({{< relref "templates-and-registries/using-an-oci-registry/appending-archives.md#remove-a-file-with-a-whiteout" >}}) a file in the archive you pull). Pro Tip: Pull an OCI tag onto a VM that already has an Anka Registry tag on it, and you can revert to it whenever you mess up the OCI tags and want a clean slate.
