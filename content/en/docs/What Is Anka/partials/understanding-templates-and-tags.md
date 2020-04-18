Anka VM “Templates” contain the base macOS installation and are optimized to minimize disk usage.

> You can create Templates using `anka create`.

> Our getting started guide has some great information on scripting the download of a macOS installer package.

After a Template is created, you can use `anka view` and `anka run` commands to install and configure whatever dependencies and software you need inside of the VM.

Once you've installed everything you need in a VM, you can then use `anka suspend` to save the state of the VM.

> Saving it as suspended allows for quick boot when you run `anka start`.

You can then push the Template to the Registry using `anka registry push` and set the Tag.

> Our use of Tags is similar to docker.

Tags contain only the delta from the base Template or previous Tag, saving a lot of disk space and bandwidth. They can be built in a sequence from the base Template, each new Tag receiving the contents of the last Tag.

If you're interested in _Infrastructure as Code_ to automate the creation of your Templates and Tags, you have several options:

1. Write scripts that:
    - Parse and iterate commands, in order, from a data serialization language like JSON and execute them inside of the VM before suspending and pushing to the Registry.
    - Or execute commands on the VM from within the script itself ([example](https://gist.github.com/NorseGaud/b637dc9c2b18116a48a040c825b16a47)).
2. Use the Packager Builder: https://github.com/veertuinc/packer-builder-veertu-anka