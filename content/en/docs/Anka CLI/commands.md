
---
date: 2019-12-12T00:00:00-00:00
title: "Anka Commands"
linkTitle: "Command Reference"
weight: 4
description: >
  Anka package provides a complete set of CLI interface to manage macOS infrastructure as a cloud for CI purposes.  
---


## Getting help
Execute `anka help` to see a complete list CLI commands available.

```
anka --help
Usage: anka [OPTIONS] COMMAND [ARGS]...

Options:
  --machine-readable              JSON output format
  --log-level [debug|info|error]
  --debug
  -l, --login TEXT                Specify the vm policy user (if configured)
  --help                          Show this message and exit.

Commands:
  clone     Clones a VM
  config    Manage Anka configuration
  create    Creates a VM
  delete    Deletes a VM (or list of vms)
  describe  Shows VM configuration
  license   licensing commands
  list      List VM library contents
  modify    Modifies a VM settings
  mount     Mounts local folder into VM
  reboot    Restarts a VM(s)
  registry  VMs registry
  run       Run commands inside VM environment
  show      Shows VM runtime properties
  start     Starts or resumes paused VM
  stop      Shuts down a vm
  suspend   Suspends a VM(s)
  unmount   Unmount shared folder (filesystem...
  usb       Do actions on USB devices
  version   prints out version
  view      Open VM display viewer
```

## Create VM
Use `anka create` command. Go to [Create VM]({{< relref "creating-a-vm.md" >}}) for more detailed instructions.

## Clone VM
Use `anka clone` command. `-c` flag consolidates/shrinks all the snapshots and creates an independent copy.
```
anka clone --help
Usage: anka clone [OPTIONS] VM_ID NEW_VM_NAME

  Clones a VM

Options:
  -c, --copy  Create independent copy instead of clone
  --help      Show this message and exit.

```
**Manage Mac host Anka Disk Space** - If you have created multiple versions or tags of a VM and pushed and pulled them on the host, then Anka caches delta associated with the tags to optimize future pulls from the registry. This can sometimes lead to excessive disk utilization on the Mac host. Use `anka clone -c` to consolidate/shrink all the snapshots and create an independent copy of the VM.



## Manage configuration
Use `anka config` command to edit Anka's configuration.
```
anka config --help
Usage: anka config [OPTIONS] [PARAM]...

  Manage Anka configuration

Options:
  -l, --list   List value parameter(s)
  -r, --reset  Reset value of parameter(s)
  --help       Show this message and exit.
```

## Delete VM
Use `anka delete` command to delete one, multiple or all VMs in the local VM directory.
```
anka delete [OPTIONS] [VMID]...

  Deletes a VM (or list of vms)

Options:
  --yes      flag. don't ask - just delete
  -a, --all  Delete all vms in library
  --help     Show this message and exit.
```

## Show VM configuration
Use `anka describe` command to view VM configuration.
```
anka describe --help
Usage: anka describe [OPTIONS] VM_ID

  Shows VM configuration

Options:
  --help  Show this message and exit.
```

## Manage anka license
Use `anka license` command to activate, remove and execute other licensing related operations for the host.
```
anka license --help
Usage: anka license [OPTIONS] COMMAND [ARGS]...

  licensing commands

Options:
  --help  Show this message and exit.

Commands:
  accept-eula  accept EULA (root privileges)
  activate     activate license key (root privileges)
  remove       removes the current license (root privileges)
  show         show license information
  validate     validates the current license
```
***Note*** To see current core consumption information for your license, use `anka license show <licensekey>`

## List of all VMs
Use `anka list` command to view all the VMs available in the local VM directory with their status and other properties.
```
anka list --help
Usage: anka list [OPTIONS] [VMID]...

  List VM library contents

Options:
  -r, --running  show only running vms
  -s, --stopped  show only stopped vms
  --help         Show this message and exit.
```


## Modify VM properties
Use `anka modify` command to change VM properties like port-forwarding, vCPU, ram, etc. You can set, add and delete properties.  

### SET Operations
```
anka modify vmname/ID set --help
Usage: anka modify set [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  cpu              set number of cpu cores and frequency
  custom-variable  configure variables
  description      set textual description of the VM
  display          configure displays
  hard-drive       modify hard drive settings
  name             set new name for the VM
  nested           enable nested virtualization
  network-card     modify network card settings
  policy           Enable VM access management (available in Anka Secure license)
  ram              set RAM size and parameters
```

**Changing hardware properties for a VM**

Use `anka modify set custom-variable` command. You can set the following custom varibales.

* boot-args                   - control the corresponding NVRAM variable  
* hw.uuid                     - specifies the Hardware UUID  
* hw.serial                   - specifies the Serial Number (system)  
* hw.manufacturer             - SMBIOS parameter (Reserved)  
* hw.product                  - SMBIOS parameter (Reserved)  
* hw.family                   - SMBIOS parameter (Reserved)  
* hw.board                    - SMBIOS parameter (Reserved)  


```
anka modify VM set custom-variable hw.UUID "GUID"
anka modify VM set custom-variable hw.serial 'MySerial'
```

### ADD Operations
```
sudo anka modify vmname/ID add --help
Usage: anka modify add [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  hard-drive
  network-card
  optical-drive
  port-forwarding
  usb-device (available in enterprise and enterprise plus tiers)
```

- #### Port Forwarding Example
  ```
  ❯ sudo anka modify 10.15.4 add port-forwarding --guest-port 22 ssh

  ❯ sudo anka describe 10.15.4
  . . .

  port_forwarding_rules

  +------------+--------------+-------------+------------+-----------+-------------+
  |   net_card |   guest_port | rule_name   | protocol   |   host_ip |   host_port |
  +============+==============+=============+============+===========+=============+
  |          0 |           22 | ssh         | tcp        |         0 |           0 |
  +------------+--------------+-------------+------------+-----------+-------------+

  ❯ sudo anka start 10.15.4
  . . . 
  port_forwarding

  +--------------+-------------+------------+--------+-----------+
  |   guest_port |   host_port | protocol   | name   | host_ip   |
  +==============+=============+============+========+===========+
  |           22 |       10000 | tcp        | ssh    | 0.0.0.0   |
  +--------------+-------------+------------+--------+-----------+

  ❯ ssh anka@localhost -p 10000
  Password:
  Last login: Mon Apr  6 12:45:50 2020
  . . .
  Mac-mini:~ anka
  ```


### DELETE Operations
```
sudo anka modify vmname/ID delete --help
Usage: anka modify delete [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  custom-variable
  hard-drive
  network-card
  optical-drive
  policy
  port-forwarding
  usb-device (available in enterprise and enterprise plus tiers)

```

## Mount local file system into the VM
Use the `anka mount` command to mount current local folder from the host inside the VM. Returns 231 on timeout.
```
anka mount --help
Usage: anka mount [OPTIONS] VM_NAME [DIRS]...

  Mounts local folder into VM

Options:
  --help  Show this message and exit.
```



## Unmount mounted folder
Use the `anka unmount` to Unmount the locally folder mounting. Returns 231 on timeout.
```
anka unmount --help
Usage: anka unmount [OPTIONS] VMID [DIR]...

  Unmount shared folder (filesystem pass-through)

Options:
  -a, --all  unmount all mounted folders
  
```

## Reboot VM
Use the `anka reboot` to reboot one, multiple or all VMs in the VM local directory.
```
anka reboot --help
Usage: anka reboot [OPTIONS] [VMID]...

  Restarts a VM(s)

Options:
  -f, --force  flag. restarts the vm process
  -a, --all    reboot all running vms
  --help       Show this message and exit.
```

## Work with registry
Use the `anka registry` command for access, push, pull VMs from the registry. See the [Manage VM Templates in Registry]({{< relref "docs/Anka Build Cloud/working-with-registry.md" >}}) Manage VM Templates in Registry documentation.  

[//]: # (TODO: broken link)


## Start VM
Use the `anka start` command to start VM. use the `-u` flag to update the guest addons inside the VM. This maybe be required for upgrading VMs for major Anka releases.
```
anka start --help
Usage: anka start [OPTIONS] VM_ID

  Starts or resumes paused VM

Options:
  -u, --update-addons, --update  Run guest addons update procedure, previous version of the addons should be already
                                 installed
  -f, --force                    Start VM with minimum checks
  -v, --view                     Open display window
  -o, --optical-drive TEXT       Path to ISO file or device
  -d, --usb TEXT                 USB device ID/Location (should be claimed)
  --help                         Show this message and exit.
```


## Stop VM
Use the `anka stop` command to stop VM.
```
anka stop --help
Usage: anka stop [OPTIONS] [VMID]...

  Shuts down a vm

Options:
  -f, --force  force the vm process to shut down
  -a, --all    Shutdown all running vms
  --help       Show this message and exit.
```


## Suspend VM
Use `anka suspend` command to put Vm in Instant Boot state. It's recommended to suspend VMs before pushing them to the Registry for use in CI.
```
anka suspend --help
Usage: anka suspend [OPTIONS] [VMID]...

  Suspends a VM(s)

Options:
  -a, --all  suspend all running vms
  --help     Show this message and exit.
```



## Open VM Window/viewer
Use `anka view` command to open running VM window. anka viewer supports Retina support for Mojave VMs.
```
anka view --help
Usage: anka view [OPTIONS] VM_ID

  Open VM display viewer

Options:
  -d, --display INTEGER  Specify the displays to view
  -s, --screenshot       Make png screenshot
  --help                 Show this message and exit.
```

## View Anka version
Use `anka version` command to view current anka version.
```
anka version
Anka Build Enterprise version 2.0 (build 108)
```

## Show running VM properties
Use `anka show` command to view VM properties of running VM.
```
anka show --help
Usage: anka show [OPTIONS] VM_ID [PROPERTY]...

  Shows VM runtime properties

Options:
  --help  Show this message and exit.
```
## Attach and Detach USB devices to VM
Use `anka USB` command to attach and work with USB devices inside the VM. The USB devices should be connected to the host. See working with [real devices]({{< relref "docs/Anka Build Cloud/using-real-devices-attached-to-anka-vms.md" >}}). 


## Anka RUN
`anka run` command provides a container like interface to work with the Anka VMs directly from the host where they are running. See [Anka RUN]({{< relref "anka-run.md" >}})

## Machine readable output
Use `--machine-readable` flag. Example `anka --machine-readable clone VMname Vmname2`. Output format is JSON.

## Debug mode
Use `--debug` flag. Example `anka --debug clone VMname Vmname2`.  


