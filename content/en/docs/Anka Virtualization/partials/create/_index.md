```shell
> sudo anka create --help
Usage: anka create [OPTIONS] VMNAME

  Creates a VM

Options:
  -m, --ram-size TEXT      Set the RAM size (including 'G' or 'M' on the end)
  -c, --cpu-count INTEGER  Set the number of CPU cores
  -d, --disk-size TEXT     Set the disk size when creating a new disk (including 'G' or 'M' on the end)
  -a, --app PATH           Path to the macOS installer .app
  -p, --pkg PATH           Include additional packages to be installed
  -s, --postinstall PATH   Include the script to run with root credentials on initial boot
  -P, --profile PATH       Include a configuration profile(s) to install alongside the macOS installation
  --interactive            Perform default interactive installation instead of the automated one
  --help                   Display usage information
```
