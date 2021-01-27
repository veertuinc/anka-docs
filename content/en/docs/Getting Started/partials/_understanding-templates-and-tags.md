> **This section mentions features (suspending, tagging, and registry push/pull) which are not available with a free Anka Develop license.**

When using Anka to create a VM, it's best to think of the result as a **VM Templates**. Post-creation from a macOS installer, Anka VM Templates contain the base/"vanilla" macOS installation.

> You can create Templates using the `anka create` command or the Anka UI just like you do with any other Anka license.

After a VM Template is created, you can use `anka view` and/or `anka run` commands to install and configure the system or whatever dependencies and software you need inside of the VM.

> [An example bash script that shows how to use `anka run` to instal dependencies](https://github.com/veertuinc/getting-started#create-vm-template-tagsbash).

You can then `anka suspend` to save the state of the VM. Suspending the VM allows for a fast boot time when running `anka start`.

> However, suspending the VM uses a fairly significant amount of disk (roughly the amount of memory you've given the VM Template). It may be better to use a "stopped" state instead to optimize disk space.

> It's very important that when you suspend from a started state the VM has **fully booted and logged into a user**. If you don't, the VM may be frozen or fail to boot. You can script this using `anka run {VMTemplateName} sleep 60` or manually check with `anka view` to ensure it's in a good state.

You can then push the Template to your Anka Build Cloud Registry using `anka registry push` and set a tag. Or, use `anka registry push -l {vmNameOrUUID} -t {tag}` to only create the Tag locally (useful if you don't have a registry yet and want to quickly switch between tags/dependencies locally while testing).

Our use of Tags is similar to docker. Each Tag contains only a new layer on top of the already existing/current VM Template layers, saving a lot of disk space and bandwidth. They can be built in a sequence from the base Template, each new Tag receiving the contents of the last Tag.

> Tags store state of the VM Template. So, if you've got Template1 and a Tag name of Tag1, then clone Template1 to Template2, Template2 will contain everything Template1/Tag1 had inside of it but no Tag on the label.

Each time you start a VM Template/Tag, it will create a new layer to store changes. This can cause disk space to increase over time, even if you delete the older versions of your dependencies inside before creating the new Tag.

However, Tags may not be the easiest and most flexible solution for you.

Instead of using Tags, VM Templates can be cloned to create a new Template. Just like Tags, this shares the previous VM Template's layers, saving space. This is the recommended approach for most users.

You have two options:

1. Use `anka clone` **without** `--copy` (recommended): The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and active Tag's state. No new disk space will be used until you start and modify that new Template.
2. Use `anka clone` **with** `--copy`: The new cloned Template will have no Tags, and will copy all of the layers from the source VM Template and active Tag. This doubles the amount of disk space used.

> When creating a clone **without** `--copy`, deletion of the first Template will not delete the contents of the second cloned Template. Anka CLI will intelligently know that the layers are still in use by the new Template.

Here is a diagram of this:

```
11.1.0 (stopped)  | 
                  | -> clone -> xcode12.3 (stopped) |
                  |                                 | -> clone -> project1 (with fastlane-v1.X) (suspended)
                  |                                 | -> clone -> project2 (with fastlane-v2.X) (suspended)
                  |
                  | -> clone -> xcode11.7 (stopped) -> clone -> project3 (suspended)
```

If you're interested in _Infrastructure as Code_ to automate the creation of your Templates and Tags, you have several options:

1. Write scripts that:
    - Sequentially parse and execute commands from a data serialization language like JSON or YAML inside of the VM.
    - Or execute commands inside of the VM from within the script itself ([example](https://github.com/veertuinc/getting-started#create-vm-template-tagsbash)).
2. Use the Packer Builder: https://github.com/veertuinc/packer-builder-veertu-anka
