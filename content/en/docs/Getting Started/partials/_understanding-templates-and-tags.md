Anka VM “Templates” contain the base macOS installation and are optimized to minimize disk usage.

> You can create Templates using `anka create`.

> Our getting started guide has some great information on scripting the download of a macOS installer package.

At this time, Anka VM Templates only support the following macOS versions:

- `10.15.x` (macOS Catalina)
- `10.14.x` (macOS Mojave)
- `10.13.x` (macOS Hi Sierra)
- `10.12.x` (macOS Sierra)
- `10.11.x` (macOS El Capitan)
- `10.10.x` (macOS Yosemite)

After a Template is created, you can use `anka view` and `anka run` commands to install and configure whatever dependencies and software you need inside of the VM.

Once you've installed everything you need in a VM, you can then use `anka suspend` to save the state of the VM.

> Saving it as suspended allows for quick boot when you run `anka start`

> Suspending (vs stopping) the VM uses more disk space as ram is saved to disk

You can then push the Template to the Registry using `anka registry push` and set the Tag. Or, use `anka registry push -l {template} -t {tag}` to only create the Tag locally (useful if you're not using a registry).

> Our use of Tags is similar to docker

Tags contain only the delta from the base Template or previous Tag, saving a lot of disk space and bandwidth. They can be built in a sequence from the base Template, each new Tag receiving the contents of the last Tag.

> Tags store state of the VM Template. So, if you've got Template1 and a Tag named Tag1 for it, then clone Template1 to Template2, Template2 will contain everything Template1/Tag1 had but not yet have a Tag of its own.

If you're interested in _Infrastructure as Code_ to automate the creation of your Templates and Tags, you have several options:

1. Write scripts that:
    - Parse and iterate commands, in order, from a data serialization language like JSON and execute them inside of the VM before suspending and pushing to the Registry.
    - Or execute commands on the VM from within the script itself ([example](https://github.com/veertuinc/getting-started/blob/master/create-vm-template-tags.bash)).
2. Use the Packer Builder: https://github.com/veertuinc/packer-builder-veertu-anka

Lastly, each Template and specific/current Tag can be cloned to create a new Template. You have two options:

1. Use `anka clone` **without** `-c` (recommended): The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and Tag's state. No new disk space will be used until you modify that new Template. The Tag you create on the new Template will only contain the delta.
2. Use `anka clone` **with** `-c`: The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and Tag's state. An independent copy of the Template and Tag will be made, in many cases doubling the disk usage.

> When creating a clone without `-c`, deletion of the first Template will not delete the contents of the second cloned Template. Anka CLI will intelligently know that the layers are still in use by the new Template.

