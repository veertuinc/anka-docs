> **This section mentions features (suspending, tagging, and registry push/pull) which are not available with a free Anka Develop license.**

When using Anka to create a VM, it's best to think of the result as a **VM "Template"**. Post-creation from a macOS installer, Anka VM Templates contain the base/"vanilla" macOS installation.

> You can create Templates using the `anka create` command or the Anka UI.

After a VM Template is created, you can use `anka view` and/or `anka run` commands to install and configure the system or whatever dependencies and software you need inside of the VM.

> [An example bash script that shows how to use `anka run` to instal dependencies](https://github.com/veertuinc/getting-started#create-vm-template-tagsbash).

You can then `anka suspend` to save the state of the VM. Suspending the VM allows for a fast boot time when running `anka start`.

> Suspending the VM uses a fairly significant amount of disk (roughly the amount of memory you've given the VM Template). It may be better to use a "stopped" state instead to optimize disk space, especially if the Template will not be directly used and instead used for cloning new VM Templates.

> It's very important that when you suspend from a started state the VM has **fully booted and logged into a user**. If you don't, the VM may be frozen or fail to boot. You can script this using `anka run {VMTemplateName} sleep 60` or manually check with `anka view` to ensure it's in a good state.

You can then push the Template to your Anka Build Cloud Registry using `anka registry push` and set a Tag. Or, use `anka registry push -l {vmNameOrUUID} -t {tag}` to only create the Tag locally (useful if you don't have a registry yet and want to quickly switch between Tags/dependencies locally while testing).

> **Important:** When you start a VM and make changes, your changes are added to a new layer on top of the existing ones. This means that when you modify a Template or Tag, then push to the registry, or even pull down a new Template or Tag to an Anka Node, you're sharing existing layers and saving disk space and bandwidth. It's important to think about which clone/tagging/layering approach is best for you and your company. _Also, each Start of a VM will cause disk space to increase, even if you delete the older versions of your dependencies inside before installing the new. It's best to delete/revert a tag or Template and clone a fresh one when you need to upgrade dependencies._

Tags may not be the easiest and most flexible solution for you. If that's the case, we recommend using cloned Templates. You have two options:

1. Use `anka clone` **without** `--copy` (recommended): The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and active Tag's state. No new disk space will be used until you start and modify that new Template.
2. Use `anka clone` **with** `--copy`: The new cloned Template will have no Tags, and will copy all of the layers from the source VM Template and active Tag. This doubles the amount of disk space used.

> When creating a clone **without** `--copy`, deletion of the first Template will not delete the contents of the second cloned Template. Anka CLI will intelligently know that the layers are still in use by the new Template.

Here is a diagram of this:

```bash
11.1.0 (stopped)  | 
                  | -> clone -> xcode12.3 (stopped) |
                  |                                 | -> clone -> project1 (with fastlane-v1.X) (suspended)
                  |                                 | -> clone -> project2 (with fastlane-v2.X) (suspended)
                  |
                  | -> clone -> xcode11.7 (stopped) -> clone -> project3 (suspended)
```

> Note the "stopped" state of the first two cloned Templates. Suspended VMs come with a fairly large state file. Unless you plan on using the vanilla 11.1.0 or xcode Templates, the suspended fast booting is only a waste of space. However, you do typically want to suspend the Templates/Tags that your CI/CD and developers will use.

If you're interested in _Infrastructure as Code_ to automate the creation of your Templates and Tags, you have several options:

1. Write scripts that:
    - Sequentially parse and execute commands from a data serialization language like JSON or YAML inside of the VM.
    - Or execute commands inside of the VM from within the script itself ([example](https://github.com/veertuinc/getting-started#create-vm-template-tagsbash)).
2. Use the Packer Builder: https://github.com/veertuinc/packer-builder-veertu-anka
