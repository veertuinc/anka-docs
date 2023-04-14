```shell
> anka create --help
usage: create [options] [name] [macOS version/installer file/installer url]

   Creates a VM Template

arguments:
  name                     VM name
  macOS version/installer file/installer url
                           MacOS version (see --list), installer file location, or installer URL to use for creation (example: 'latest')

options:
  -m,--ram-size <val>      Specify the VM RAM size (supported suffixes: T|G|M|K)
  -c,--cpu-count <val>     Specify the number of vCPU cores for the VM (3 or more is recommended)
  -d,--disk-size <val>     Specify the VM disk size (supported suffixes: T|G|M|K)
  --no-setup               Do not perform automated macOS setup
  -q,--quiet               Do not show progress
  -l,--list                List available macOS versions to install
```
