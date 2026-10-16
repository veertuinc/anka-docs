Create a new VM. `anka create` installs macOS only. Leave `push_mode` at `1` for this push. `1` is the default. This push uploads the macOS disk as a block tag to the Anka Registry in `ANKA_REGISTRY`. You can also push to an OCI registry with this mode.

```bash
❯ anka create 27.0.1 27.0.1
❯ anka push 27.0.1 -t vanilla http://anka.registry:8089
```

`http://anka.registry:8089` is the Anka Registry. An OCI registry URL has a `/v2/` path. This push creates template `27.0.1` and tag `vanilla` on the Anka Registry. A new `27.0.1` VM reports about `29.82GiB` uploaded to tag `vanilla`. Pull this tag when a host needs the macOS disk.

Set `push_mode` to `2` before you push the tool tags.

```bash
❯ anka config push_mode 2
```

{{< hint info >}}
`ANKA_PUSH_MODE` sets the mode for one command. Use it when a change to `anka config` is a problem. Example: `ANKA_PUSH_MODE=2 anka push 27.0.1 -t xcode-27.1 oci://registry.example.com/v2/`
{{< /hint >}}

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

The tag chain on this VM is now the `vanilla` tag, then the `xcode-27.1` tag, then the CMake changes. Push `cmake` with one of the two commands below. Note that `--thin` works with `push_mode` `1` and with `push_mode` `2`.
