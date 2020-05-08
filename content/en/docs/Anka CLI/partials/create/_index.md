```shell
> sudo anka create --help
Usage: anka create [OPTIONS] VMNAME

  Creates a VM

Options:
  -m, --ram-size TEXT      ram size in G  [default: 4G]
  -c, --cpu-count INTEGER  the number of cpu cores  [default: 2]
  -d, --disk-size TEXT     sets the disk size when creating a new disk, G/M suffix needed  [default: 80G]
  -a, --app PATH           Path to Install macOS Application (downloadable from AppStore)
  -p, --pkg PATH           Additional package to be installed
  -s, --postinstall PATH   Postinstall scripts (to run with root credentials at first boot)
  -P, --profile PATH       Install configuration profile(s) alongside macOS installation
  --interactive            Perform default interactive installation instead of the automated one
  --help                   Show this message and exit.
```
