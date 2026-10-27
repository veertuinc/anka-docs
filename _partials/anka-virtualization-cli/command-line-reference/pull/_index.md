```shell
> anka pull --help
usage: pull [options] vmid [remote]

   Pull a VM template from the registry

arguments:
  vmid                     VM to pull
  remote                   Remote registry/template to pull

options:
  -t,--tag <val>           Pull the particular tag (latest if not specfied)
  -l,--local               Checkout (make it current) local tag
  --fetch-only             Download a tag without checkout
  -s,--shrink              Delete other local tags to optimize disk usage
  --check-download-size    Get the tag size only
  --transform <val>        Use sed replace expression to transform file names
  -q,--quiet               Do not show progress
  -n,--dry-run             Just show files being pulled
  --thin                   Pull only latest layer specified by tag without base
```
