Install one tool, stop the VM, and push the next tag with `--thin`. Leave `push_mode` at `1`. Leave the repository name off the URL. Anka uses the VM name.

```bash
❯ anka start 27.0.1
# install Xcode in the VM, then stop it
❯ anka stop 27.0.1
❯ anka push --thin 27.0.1 -t xcode-27.1 oci://registry.example.com/v2/
```

The `xcode-27.1` tag is a thin tag. It holds only the last layer: the files that changed after `vanilla`.

Install CMake and stop the VM.

```bash
❯ anka start 27.0.1
# install CMake in the VM, then stop it
❯ anka stop 27.0.1
```

The tag chain on this VM is now the `vanilla` tag, then the `xcode-27.1` tag, then the CMake changes. Push `cmake` with `--thin` so that tag holds only the CMake layer. `push_mode` `2` is another way to store only the file changes. See [Push modes]({{< relref "templates-and-registries/templates-and-tags.md#push-modes" >}}).
