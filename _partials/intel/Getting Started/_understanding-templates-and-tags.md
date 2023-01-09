> **This section mentions features (suspending, tagging, and registry push/pull) which are not available with a free Anka Develop license.**

When using Anka to create a VM, it's best to think of the result as a **VM "Template"**. A "template" should suggests to you that it is something you don't directly use. This will make more sense later.

To create a VM Template, you'll use the Anka CLI's `create` command or Anka UI with any macOS installer .app file (Anka 2/Intel). After a VM Template is created, you'll have a clean macOS installation inside of the VM with some small tweaks for Anka to function. You can use `anka view` and/or `anka run` commands to install dependencies and software you need inside of the VM.

{{< hint warning >}}
[An example bash script that shows how to use `anka run` to install dependencies](https://github.com/veertuinc/getting-started#create-vm-template-tagsbash).
{{< /hint >}}

Once the VM Template is configured how you want it, you have two options:

1. Stop the VM Template: `anka stop {templateName}`
2. (intel only) Suspend the VM Template: `anka suspend {templateName}`

With option 1, you are issuing a full shutdown -- similar to hitting the power button on your physical machine -- and therefore have to wait the full boot time for the VM to start back up. This usually takes 20 to 30 seconds.

With option 2, you can save the state of the VM/memory to a file which can then be used to start the VM almost instantly. However, these files that contain the state are several GBs in size and shouldn't be used if your hosts are limited on disk space. Stopped Big Sur VMs are ~19GB, but can be more than 30GBs depending on the amount of memory given to the VM.

{{< hint warning >}}
It's very important that when you suspend from a started state the VM has **fully booted and logged into a user**. If you don't, the VM may be frozen or fail to boot. You can script this using `anka run {VMTemplateName} sleep 60` or manually check with `anka view` to ensure it's in a good state.
{{< /hint >}}

The VM Template is now ready to be used. You shouldn't directly use it to run your jobs as this will cause the VM Template to become "dirty". You will instead use `anka clone {VMTemplateName} job-1-vm` to create a new VM from the template. Any changes made inside of the cloned VM will not impact the original VM Template. You can then delete the clone once you're done with it.

It's important that you store this VM Template in a location that allows other hosts with Anka installed to pull it and use it. You can push the template to your Anka Build Cloud Registry using `anka registry push` and set a Tag. Or, use `anka registry push -l {vmNameOrUUID} -t {tag}` to only create the Tag locally (useful if you don't have a registry yet and want to quickly switch between Tags/dependencies locally while testing).

{{< hint info >}}

**Important:** Each time you clone a VM you're creating a new layer/file to store the file system changes inside of the VM. However, any data needed to run the VM from previous layers is not added to this new layer and it instead just references the existing layers/files on disk. This optimizes disk space (like if you have several clones from the same source, they all share as many underlying layers as possible). However, because the new layer/file for the VM stores all changes made inside of the VM post-clone, each new block written has no access to previous blocks, layers, etc. Even if it does have access because they are inside of the new layer, those blocks are not fully deleted from the layer. This causes long running VMs and also repetitive manual changes to the same VM Template to pile up and exhaust disk space on the host. Both of these problems should be considered carefully when designing your VM creation, cloning, and modification approach. We will recommend the best approach below.

{{< /hint >}}

Once a Template is created, and Tag assigned to it, you can now create other Templates/Tags from it. Fortunately, Anka allows you to build Templates on top of each other and share the underlying files between them, cutting down on the disk space requirements. This makes switching between dependencies/VM Templates easy and also saves on bandwidth if layers for the VM Template already exist on the host.

You have two options to create VM Templates from VM Templates:

1. Use `anka clone {sourceTemplate} {newTemplate}` (recommended): The new cloned Template will have no Tags, but will link to the layers/contents from the previous Template and active Tag's state. No new disk space will be used until you start and make changes.
2. Use `anka clone --copy {sourceTemplate} {newTemplate}`: The new cloned Template will have no Tags, and will copy all of the parent Template's layers into a single file ("flattening"). This doubles the amount of disk space used.

> When creating a clone **without** `--copy`, deletion of the first Template will not delete the contents of the second cloned Template. Anka CLI will intelligently know that the layers are still in use by the new Template.

Here is a diagram of a common and recommended pattern for creating Templates/Tags:

```bash
11.1.0 (stopped)  | 
                  | -> clone -> xcode12.3 (stopped) |
                  |                                 | -> clone -> project1 (with fastlane-v1.X) (suspended)
                  |                                 | -> clone -> project2 (with fastlane-v2.X) (suspended)
                  |
                  | -> clone -> xcode11.7 (stopped) -> clone -> project3 (suspended)
```

{{< hint info >}}
Note the "stopped" state of the first two cloned VM Templates. Unless you plan on using the vanilla 11.1.0 or xcode VM Templates, the suspended state is a waste of space. However, you do typically want to suspend the final/cloned Templates/Tags that your CI/CD and developers will use.
{{< /hint >}}

You can also create multiple tags on top of a single VM Template. This is done by simply starting the VM, stopping/suspending once changes are made, and then pushing to the registry with a new Tag. However, this causes problems when trying to quickly switch between tags for different projects, as the Anka Build Cloud only allows a single Tag on a machine at once (not a problem if manually executing `anka pull` though). This means that switching would cause a download to happen, and depending on the size of the other tag, could slow down your CI/CD considerably. This is where cloned VM Templates shine. Multiple Templates for multiple projects can exist at once on a host, all sharing underlying layers and cutting down on disk space usage and time to start VMs for your CI/CD jobs.

If you're interested in _Infrastructure as Code_ to automate the creation of your Templates and Tags, you have several options:

1. Write scripts that:
    - Sequentially parse and execute commands from a data serialization language like JSON or YAML inside of the VM.
    - Or execute commands inside of the VM from within the script itself ([example](https://github.com/veertuinc/getting-started#create-vm-template-tagsbash)).
2. Use the Packer Builder: https://github.com/veertuinc/packer-builder-veertu-anka
