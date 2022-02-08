```shell
> anka create --help
usage: create [options] name

   Creates a VM Template

arguments:
  name                     VM name

options:
  -m,--ram-size <val>      Specify the VM RAM size (supported suffixes: T|G|M|K)
  -c,--cpu-count <val>     Specify the number of vCPU cores for the VM (3 or more is recommended)
  -d,--disk-size <val>     Specify the VM disk size (supported suffixes: T|G|M|K)
  -a,--app <val>           Path or URL (or 'latest' to dowload the latest version) to the macOS restore image
```
