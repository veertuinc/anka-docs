```shell
> anka pull --help
usage: pull [options] vmid [remote]

   Pull a VM template from the registry

arguments:
  vmid                     VM to pull
  remote                   Sets an alternate registry

options:
  -t,--tag <val>           Pull the particular tag (latest if not specfied)
  -l,--local               Checkout (make it current) local tag
  --fetch-only             Download tag without checkout
  -s,--shrink              Delete other local tags to optimize disk usage
  --check-download-size    Get the tag size only
  -q,--quiet               Do not show progress
```
