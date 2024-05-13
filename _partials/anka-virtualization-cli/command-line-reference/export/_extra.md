---
---

- Only available for Enterprise or Enterprise Plus licenses.
- Exporting a VM is performing a unique copy like `anka clone -c` would do if it has no tags.
- If you export a tagged VM in a chain (a VM that was cloned from another, or a tag on top of another VM tag), it will only export the VM layers that relate to that tag. You have to export and import all tags in the order they were created. This adds a fair bit of complexity though as each VM Tag is its own separate layer. When exporting, you'll need to ensure the target is active on the host. To make it active, you can `anka pull --tag 1 templateA` (`--local` may speed this up if it was already pulled). Once active, exporting it requires `--tag <val>`. 

{{< hint warning >}}
This feature does not work if `chunk_size` is in use for the VM templates/tags.
{{< /hint >}}
