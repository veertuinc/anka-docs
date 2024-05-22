---
date: 2024-02-27T01:00:00-00:00
title: "Anka Virtualization 3.3.9"
---

We are very excited to announce Anka Virtualization 3.3.9. In this version, you're going to find:

## Ability to set VM swap location to a specific host level location

{{< hint warning >}}

This feature is for ARM/Silicon only.

{{< /hint >}}

This feature was designed for EC2/EBS users to be able to use the internal SSD on the host for VM swap space and avoid any performance limitations of EBS when VMs run out of Memory under heavy usage. It's technically not just for EBS/EC2 users though. You can set `anka config swap_dir` (or the ENV) to anything you want on any arch/hardware and it will automatically mount a directory from there into the VM.

- To enable for all VMs, set `[sudo] anka config swap_dir`. If not set, it is disabled.
- Each VM's swap mount is isolated from the others. They are stored as disk images under the location you specify for swap_dir.
- It can be disabled per VM by using `ANKA_SWAP_DIR="" anka start . . .`
- It can be enabled per VM by using `ANKA_SWAP_DIR="/location/on/host" anka start . . .`
- Once mounted, it will automatically update `sudo sysctl vm.swapfileprefix` to the proper path. No action is necessary inside of the VM to use it. However, the feature will only work with 3.3.9 addons installed in the VM.

## Export and Import v2

In Anka 3.3.8 we introduced v1 of our export and import CLI commands. These commands did not consider VM Tags though, which was quite limiting if users wanted to migrate the VM between hosts, but maintain the Tags in order to optimize any subsequent pulls that happened.

This adds a fair bit of complexity though as each VM Tag is its own separate layer. When exporting, you'll need to ensure the target is active on the host. To make it active, you can `anka pull --tag 1 templateA` (`--local` may speed this up if it was already pulled). Once active, exporting it requires `--tag <val>`.

However, when importing, you will need to import all previous tags that make up the tag you wish to import. If we have `templateA` with three tags: `1` (the base containing all of the data needed to run macOS), `2`, and `3`, you will need to import tags 1 and 2 in order to import `3`. However, this is not needed if tag `2` is only a config change and the VM was not started/new data was not written inside of the VM layers.

In short, your exporting/importing must be done in the order of tag creation if those tags are dependent on each other.

{{< hint warning >}}
This feature does not work if `chunk_size` is in use for the VM templates/tags.
{{< /hint >}}
