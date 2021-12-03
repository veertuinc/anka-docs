```shell
> anka pull --help
usage: pull [options] vmid

   Pull a VM template from the registry

options:
  -t,--tag <val>           Pull the particular tag (latest if not specfied)
  -l,--local               Checkout (make it current) local tag
  --fetch-only             Download tag without checkout (making it current)
  -s,--shrink              Delete other local tags to optimize disk usage
  --check-download-size    
  -q,--quiet               Do not show progress
  -r,--regisry <val>       Specifies an alternate source of a pull operation
```
