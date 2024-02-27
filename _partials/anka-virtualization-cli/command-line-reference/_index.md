```shell
> anka  --help
usage: anka [options] <command>

   Anka command line tool

options:
  -j,--machine-readable    Output a machine readable format (JSON)
  --version                Output the Anka version

commands:
  list                     List local VM library
  config                   Manage the CLI configuration
  show                     Show a VM's properties
  modify                   Modify a VM parameters
  view                     Open VM display
  run                      Run a command inside of a VM
  create                   Creates a VM Template
  start                    Start or resume a VM
  stop                     Shut down a VM(s)
  clone                    Clone a VM
  push                     Push a VM to the registry
  pull                     Pull a VM template from the registry
  usb                      Manage USB devices
  attach                   Attach USB device(s) to a running VM
  detach                   Detach USB device(s) from a VM
  reboot                   Reboot a running VM(s)
  delete                   Delete a VM(s) and tags
  registry                 Configure and control template registries
  cp                       Copy files in/out of the Anka VM and host:
       cp [options] <vmid>:path/inside </path/on/host>
       cp [options] </path/on/host> <vmid>:[path/inside]
  license                  Manage licenses
  export                   Export a VM as an archive
  import                   Import VM from an archive
```
