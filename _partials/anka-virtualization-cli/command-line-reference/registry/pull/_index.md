```shell
> anka registry pull --help
usage: pull [options] vmid

   Pull a VM template from the registry

arguments:
  vmid                     VM to pull

options:
  -t,--tag <val>           Pull the particular tag (latest if not specfied)
  -l,--local               Checkout (make it current) local tag
  -s,--shrink              Delete other local tags to optimize disk usage
```
