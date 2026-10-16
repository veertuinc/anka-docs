`anka export` and `anka import` move a tag between hosts when you do not use a registry. Export has no `--thin` flag. With `push_mode` `1`, export of the first tag writes the VM. That tag has no parent tag. Import of that archive creates a VM that boots and keeps the tag. With `push_mode` `2`, export writes the diff for the tag you name. The `vanilla` diff is empty, so that export does not include the macOS disk. `xcode-27.1` exports the Xcode files. `cmake` exports the CMake files. `anka export -c` does not add the base tag while `push_mode` is `2`. Import with `-v` applies that diff onto a VM that already exists.

```bash
❯ anka export 27.0.1 xcode-27.1.tar -t xcode-27.1
❯ anka import xcode-27.1.tar -v 15.6.1
```

`15.6.1` must already exist. Import applies that tag's layer onto it. Import the tags in the order you created them.

[Exporting and importing VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md" >}}) describes the rest of the command, including the license requirement.
