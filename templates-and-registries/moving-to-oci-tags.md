---
title: "Moving to OCI tags"
linkTitle: "Moving to OCI tags"
weight: 6
description: >
  Move the tool layers of your Anka Registry templates to OCI tags, one step at a time
---

You do not have to replace the Anka Registry to use [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}). We recommend this approach:

- Keep the full templates in the Anka Registry.
- Move only the tool layers of each template, such as Xcode, Homebrew, and language runtimes, to OCI tags.
- Build each final template on a producer host and push it back to the Anka Registry with the same name.

The Build Cloud Controller, your CI plugins, and your template names stay the same. The tools become small layers that you can pull onto more than one macOS version.

## Should you move?

- **You use Anka Build Cloud:** move only the tool layers. The Controller pulls templates only from the Anka Registry, so the final templates must stay there.
- **You use only the Anka CLI and have a container registry:** you can move all templates, including the macOS base, to your OCI registry. After that, you do not need the Anka Registry.
- **You have one or two templates that do not change often:** you get little from the change. Stay on the Anka Registry.

## Map your template hierarchy to OCI tags

Many teams use the [clone hierarchy that we recommend for templates]({{< relref "anka-build-cloud/getting-started/registry-vm-templates-and-tags.md" >}}): a vanilla macOS VM, then a clone with Homebrew and git, then a clone with Xcode, then one clone for each project. Move each step as follows:

| Step in the hierarchy | Where it goes |
|-----------------------|---------------|
| vanilla macOS | A full tag. Keep it in the Anka Registry, or push it to an OCI registry. Leave `push_mode` at `1`. Do not use `--thin` on this push. |
| Homebrew and git | An OCI tag. |
| Port forwarding, CPU, RAM, and other VM settings | The base template, or `EXPOSE`, `LABEL`, and `MOUNT` in an [Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}). An OCI tag holds files. Do not use it for VM settings. |
| Xcode | One OCI tag for each Xcode version. You can use one Xcode tag with any base template, including base templates that you make in the future. Of course, the macOS version of the base template must support that Xcode version. |
| Project dependencies | An OCI tag when more than one template uses them. If not, keep them in the final template. |

## Move in phases

### Phase 1: Upgrade with no change in behavior

Install Anka 3.10.0 on your producer/builder hosts first. Leave `anka config push_mode` at `1` (the default). Your current push and pull scripts work the same. Use `--thin` later, when you push a tool as an OCI tag.

Note: Anka Nodes that run VMs need Anka 3.10.0 only when they pull OCI tags.

### Phase 2: Make your first tool tag from a clean base

[Set up access to your OCI registry]({{< relref "templates-and-registries/using-an-oci-registry/_index.md" >}}). Then clone your vanilla VM, mark it with a local tag, install one tool, and push only the last layer with `--thin`:

```bash
❯ anka clone 26.4.1 xcode-builder
❯ anka push --local --tag base xcode-builder
❯ anka start xcode-builder
# install Xcode in the VM, then stop it
❯ anka stop xcode-builder
❯ anka push --thin xcode-builder -t xcode-26.1 oci://registry.example.com/v2/xcode
```

The `xcode-26.1` tag holds only the files that changed after the local `base` tag. You do not change `push_mode`.

{{< hint info >}}
When you push with `--thin`, Anka does not push the paths in the `ankaignore` file, such as caches and logs. To exclude more paths from your tool tag, see [Exclude files from a push]({{< relref "templates-and-registries/templates-and-tags.md#exclude-files-from-a-push" >}}).
{{< /hint >}}

{{< hint info >}}
Install each tool again on a clean clone. Do not try to take it out of an existing Anka Registry tag. An existing tag often holds more than one change, and also caches and logs.
{{< /hint >}}

Pull the new tag onto a clone of your vanilla VM and run your tests before you go to the next phase:

```bash
❯ anka clone 26.4.1 xcode-test
❯ anka pull xcode-test oci://registry.example.com/v2/xcode:xcode-26.1
```

### Phase 3: Build your final templates from the base and the OCI tags

On a producer host, pull the base template from the Anka Registry. Pull the OCI tags in order. Push the result to the Anka Registry with the existing template name. The Anka Registry stores it as a full tag, and the Controller uses it like any other template.

{{< hint warning >}}
Do not use `--thin` on this push. The finished template must include the macOS disk. Leave `push_mode` at `1`.
{{< /hint >}}

See [Preparing a build VM]({{< relref "templates-and-registries/preparing-a-build-vm.md" >}}) for the commands.

Keep the pull order in an [Ankafile]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-an-ankafile.md" >}}) in your source control. Each change to a template then goes through code review, and you can build the same template again at any time.

### Phase 4 (optional): Remove the Anka Registry

This phase is only for teams that do not use Anka Build Cloud. Push the macOS base to your OCI registry. Leave `push_mode` at `1`, and do not use `--thin` on that push. Then let each runner create its VM from the Ankafile, or pull the finished template from the OCI registry.

## Rules to follow

- **Pull the Anka Registry tag first.** Then pull the OCI tags. A pull of an Anka Registry tag removes the OCI tags that you pulled before it. See [Anka Registry tags and OCI tags]({{< relref "templates-and-registries/templates-and-tags.md#anka-registry-tags-and-oci-tags" >}}).
- **Use `--thin` for tool tags.** Leave `push_mode` at `1`. Do not use `--thin` when you push a full template. A full template pushed with `--thin` or with `push_mode` `2` does not include the macOS disk.
- **Use tag names that do not change.** Use names such as `xcode-26.1`, not `xcode-latest`. The Anka Registry gives each new tag a new number. An OCI registry replaces a tag when you push the same tag name again.
- **Record the minimum macOS version.** An OCI tag applies to any VM, but the tool in it can need a specific macOS version. Put the version in the tag name or the tag description.
- **Remove files with a whiteout.** To remove a file from an OCI tag, [append an archive with a whiteout]({{< relref "templates-and-registries/using-an-oci-registry/appending-archives.md#remove-a-file-with-a-whiteout" >}}). You do not rebuild the earlier layers.
- **Refresh registry credentials in CI.** Some registry credentials expire. For example, Amazon ECR credentials expire after about 12 hours.

## Go back to the old templates

In Phases 2 and 3, your old templates stay in the Anka Registry, and nothing changes them. To go back, point your CI jobs at the old tag.

On a VM, pull the Anka Registry tag again to get a clean VM. When you delete a local OCI tag, the files from that tag stay on the VM.
