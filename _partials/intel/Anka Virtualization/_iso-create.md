
### Using an ISO file to create Anka VM - For Yosemite & El Capitan VMs only

Use `create_macos_install_image.sh` included in the Anka package to first create an `.iso` from your Yosemite and ElCapitan `.app` installer.
If you already have `.iso` file, you don't need to execute this step.

```shell
/Library/Application\ Support/Veertu/Anka/tools/create_macos_install_image.sh
Usage: create_macos_install_image.sh install_macos.app [OPTIONS]...

Options:
--g,--guest-addons       Embed Anka guest addons in the installer
--o,--output output.iso       Specify output image file, if not specified the image will be created in working directory
--p,--pkg path/to/pkg Specify additional packages to include into the installer
```

For example:

```shell
/Library/Application\ Support/Veertu/Anka/tools/create_macos_install_image.sh /Applications/Install\ macOS\ Sierra.app
```

To create a VM from `.iso`, you will use [`anka create`]({{< relref "intel/Anka Virtualization/command-reference.md#create" >}}) command as you typically would. It will create an empty VM.
Note - > While creating VM with anka create, make sure to specify enough --disk-size parameter. Currently, it's not possible to change the disk size for an existing VM.

```shell
sudo anka create --ram-size 2G --cpu-count 2 --disk-size 60G sierravm
vm created successfully with uuid: dfaa97c5-2154-11e8-881d-acbc32ad1d59
```

Then, start the VM with the sierra ISO attached:

```shell
sudo anka start --view -o sierra.iso sierravm
+-----------------------+--------------------------------------+
| uuid                  | dfaa97c5-2154-11e8-881d-acbc32ad1d59 |
| name                  | sierravm                             |
| cpu_cores             | 2                                    |
| ram                   | 2G                                   |
| hard_drive            | 60 GB (11.2 MB on disk)              |
| addons_version        | not found                            |
| status                | running                              |
| vnc_connection_string | vnc://:admin@10.0.1.12:5900          |
| view_vm_display       | anka view sierravm                   |
+-----------------------+--------------------------------------+
```

Complete the macOS setup inside the VM. Then, stop the VM.

Start the VM again with guest addons ISO installed.

```shell
sudo anka start -v -o /Library/Application\ Support/Veertu/Anka/guestaddons/anka-addons-mac.iso sierravm
```

Complete the guest addons installation inside the VM. Shutdown the VM with `sudo anka stop {vmNameOrUUID}`.

Validate by running the following command `sudo anka run {vmNameOrUUID} ls -l` from the host. It should display ls -l contents of the VM. The VM is correctly created.