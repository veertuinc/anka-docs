Create a new VM. `anka create` installs macOS only. Leave `push_mode` at `1` for this push. `1` is the default. This push uploads the macOS disk as a full tag to the OCI registry.

```bash
❯ anka create 27.0.1 27.0.1
❯ anka push 27.0.1 -t vanilla oci://registry.example.com/v2/
```

An OCI registry URL has a `/v2/` path. Leave the repository name off the URL. Anka uses the VM name. This push creates repository `27.0.1` and tag `vanilla`. A new `27.0.1` VM reports about `29.82GiB` uploaded to tag `vanilla`. Pull this tag when a host needs the macOS disk:

```bash
❯ anka pull 27.0.1 oci://registry.example.com/v2/27.0.1:vanilla
```
