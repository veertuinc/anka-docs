The order of the OCI tags matters on the producer host. Create the VM, then pull each OCI tag in that order. Push the finished VM as a template.

```bash
❯ anka create build_vm 27.0.1
❯ anka pull build_vm http://registry.example.com/v2/xcode:27.1
❯ anka pull build_vm https://ghcr.io/homebrew/core/cmake:4.4.4
❯ anka push build_vm -t latest
```

`anka push` uses the registry in `ANKA_REGISTRY`. To use a different registry, add the registry URL:

```bash
❯ anka push build_vm -t latest http://registry.example.com/v2
```

A runner host does not have the OCI tag order. It does not know which OCI tags the producer used. It pulls the template and starts it. That registry can be the Anka Registry. It can also be an OCI registry.

```bash
❯ anka pull build_vm
❯ anka start build_vm
```
