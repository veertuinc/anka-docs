A later push of that VM with `push_mode` `2`, after one `--thin` pull, stores two file layers. One layer is the changes already on the VM. The other layer is the thin tag. The base tag, such as `vanilla`, stays out.

```bash
❯ anka push 15.6.1 -t cmake-on-15 oci://registry.example.com/v2/
❯ anka -j registry show oci://registry.example.com/v2/15.6.1:cmake-on-15 --manifest
```
