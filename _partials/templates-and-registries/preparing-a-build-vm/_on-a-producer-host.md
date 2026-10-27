The order of the thin tags matters on the producer host. Start from a new VM, or from a vanilla full tag that you already have. Then pull each thin tag with `--thin`, in that order. Push the finished VM as a full tag. Do not use `--thin` on that last push.

Create a new VM:

```bash
❯ anka create build_vm 27.0.1
❯ anka pull --thin build_vm http://registry.example.com/v2/xcode:27.1
❯ anka pull --thin build_vm https://ghcr.io/homebrew/core/cmake:4.4.4
❯ anka push build_vm -t latest
```

Or pull a vanilla tag. Use this when the macOS base is already in a registry:

```bash
❯ anka pull 27.0.1 -t vanilla
❯ anka pull --thin 27.0.1 http://registry.example.com/v2/xcode:27.1
❯ anka pull --thin 27.0.1 https://ghcr.io/homebrew/core/cmake:4.4.4
❯ anka push 27.0.1 -t latest
```

To pull that vanilla tag from an OCI registry instead:

```bash
❯ anka pull 27.0.1 oci://registry.example.com/v2/27.0.1:vanilla
```

`anka push` uses the registry in `ANKA_REGISTRY`. To use a different registry, add the registry URL:

```bash
❯ anka push build_vm -t latest http://registry.example.com/v2
```

{{< hint info >}}
Leave `push_mode` at `1` (the default). Use `--thin` when you pull each thin tag. `--thin` on the final push would leave the macOS disk out, and a runner could not start the template. A push to the Anka Registry always includes the macOS disk.
{{< /hint >}}

A runner host does not have the thin tag order. It does not know which thin tags the producer used. It pulls the template as a full tag and starts it. That registry can be the Anka Registry. It can also be an OCI registry.

```bash
❯ anka pull build_vm
❯ anka start build_vm
```
