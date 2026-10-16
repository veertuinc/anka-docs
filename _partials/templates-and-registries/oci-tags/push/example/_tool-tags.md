Set `push_mode` to `2` before you push the tool tags.

{{< include file="_partials/templates-and-registries/oci-tags/push/_set-push-mode.md" >}}

Install one tool, stop the VM, and push the next tag to the OCI registry. Leave the repository name off the URL. Anka uses the VM name.

```bash
❯ anka start 27.0.1
# install Xcode in the VM, then stop it
❯ anka stop 27.0.1
❯ anka push 27.0.1 -t xcode-27.1 oci://registry.example.com/v2/
```

The `xcode-27.1` push uploads only the file changes since `vanilla`.

Install CMake and stop the VM.

```bash
❯ anka start 27.0.1
# install CMake in the VM, then stop it
❯ anka stop 27.0.1
```

The tag chain on this VM is now the `vanilla` tag, then the `xcode-27.1` tag, then the CMake changes. You can push `cmake` without `--thin` or with `--thin`. Note that `--thin` works with `push_mode` `1` and with `push_mode` `2`.
