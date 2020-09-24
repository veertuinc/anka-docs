> **This section mentions features (suspending, tagging, and registry push/pull) which are not available with a free Anka Develop license.**

When using Anka Build to create a VM, it's best to think of Anka VMs as **VM Templates**. Anka VM Templates contain the base macOS installation with a few modifications for optimal performance. You can create Templates using the `anka create` command or the Anka UI just like you do with any other Anka license.

After a Template is created, you can use `anka view` and/or `anka run` commands to install and configure the system or whatever dependencies and software you need inside of the VM.

You can then `anka suspend` to save the state of the VM. Suspending the VM allows for a fast boot time when running `anka start`. Suspending the VM uses a fairly significant amount of disk (roughly the amount of memory you've given the VM Template.

> It's very important that when you suspend from a started state that the VM has fully booted and logged into a user. If you don't, the VM will fail to start.

You can then push the Template to your Anka Build Cloud Registry using `anka registry push` and set the Tag. Or, use `anka registry push -l {vmNameOrUUID} -t {tag}` to only create the Tag locally (useful if you don't have a registry yet and want to quickly switch between tags/dependencies).

Our use of Tags is similar to docker. Tags contain only the delta from the base Template or previous Tag, saving a lot of disk space and bandwidth. They can be built in a sequence from the base Template, each new Tag receiving the contents of the last Tag.

> Tags store state of the VM Template. So, if you've got Template1 and a Tag name of Tag1, then clone Template1 to Template2, Template2 will contain everything Template1/Tag1 had inside of it. Post-clone, it will not have any Tags associated with it though.

If you're interested in _Infrastructure as Code_ to automate the creation of your Templates and Tags, you have several options:

1. Write scripts that:
    - Sequentially parse and execute commands from a data serialization language like JSON or YAML inside of the VM.
    - Or execute commands inside of the VM from within the script itself ([example](https://github.com/veertuinc/getting-started/blob/master/ANKA_BUILD_CLOUD/create-tags.bash)).
2. Use the Packer Builder: https://github.com/veertuinc/packer-builder-veertu-anka

Lastly, existing VM Templates can be cloned to create a new Template. You have two options:

1. Use `anka clone` **without** `--copy` (recommended): The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and Tag's state. No new disk space will be used until you modify that new Template. The Tag you create on the new Template will only contain the delta.
2. Use `anka clone` **with** `--copy`: The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and Tag's state. An independent copy of the Template and Tag will be made, in many cases doubling the disk usage.

> When creating a clone without `--copy`, deletion of the first Template will not delete the contents of the second cloned Template. Anka CLI will intelligently know that the layers are still in use by the new Template.

