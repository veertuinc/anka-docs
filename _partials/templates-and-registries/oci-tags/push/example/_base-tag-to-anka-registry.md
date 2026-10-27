Create a new VM. `anka create` installs macOS only. This push uploads the macOS disk as a full tag to the Anka Registry in `ANKA_REGISTRY`. The Anka Registry always stores the full tag, so `push_mode` does not apply.

```bash
❯ anka create 27.0.1 27.0.1
❯ anka push 27.0.1 -t vanilla http://anka.registry:8089
```

`http://anka.registry:8089` is the Anka Registry. An OCI registry URL has a `/v2/` path. This push creates template `27.0.1` and tag `vanilla` on the Anka Registry. A new `27.0.1` VM reports about `29.82GiB` uploaded to tag `vanilla`. Pull this tag when a host needs the macOS disk:

```bash
❯ anka pull 27.0.1 -t vanilla http://anka.registry:8089
```
