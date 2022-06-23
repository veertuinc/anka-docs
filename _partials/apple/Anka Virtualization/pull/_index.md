```shell
> anka pull --help
usage: pull [options] vmid

   Pull a VM template from the registry

arguments:
  vmid                     VM to pull

options:
  -t,--tag <val>           Pull the particular tag (latest if not specfied)
  -l,--local               Checkout (make it current) local tag
  --fetch-only             Download tag without checkout
  -s,--shrink              Delete other local tags to optimize disk usage
  --check-download-size    Get the particular tag size (latest if not specfied)
  -q,--quiet               Do not show progress
  -r,--registry <val>      Sets an alternate registry
```
