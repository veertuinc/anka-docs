

---
date: 2019-07-03T22:24:47-05:00
title: "Create VM"
linkTitle: "Create VM"
weight: 3
description: >
  Creating an Anka VM with Anka Flow.
---



## Create VM
Anka makes it very simple manage your macOS CI infrastructure-as-a-code.

Use [`anka create`]({{< relref "docs/Anka CLI/command-reference.md#create" >}}) command to create macOS VMs from the `.app` installer app.
`anka create --ram-size 4G --cpu-count 2 --disk-size 80G --app /Applications/Install\ macOS\ High\ Sierra.app Hisierravm`

***Note*** For Catalina Anka VMs, --ram-size value should be 4G and --disk-size should be 80G.

By default [`anka create`]({{< relref "docs/Anka CLI/command-reference.md#create" >}}) creates macOS VM with administrative `user - anka and password - admin`. You can change this default user by using these ENV variables with [`anka create`]({{< relref "docs/Anka CLI/command-reference.md#create" >}}) command.

`ANKA_DEFAULT_PASSWD=passwd ANKA_DEFAULT_USER=usrname anka create --ram-size 4G --cpu-count 2 --disk-size 60G -a /Applications/Install\ macOS\ High\ Sierra.app HiSierravm`

```
anka create [OPTIONS] {template}

  Creates a VM

Options:
  -m, --ram-size TEXT      ram size in G  [default: 4G]
  -c, --cpu-count INTEGER  the number of cpu cores  [default: 2]
  -d, --disk-size TEXT     sets the disk size when creating a new disk, G/M suffix
                           needed  [default: 80G]
  -a, --app PATH           Path to Install macOS Application (downloadable from
                           AppStore)
  -p, --pkg PATH           Additional package to be installed
  -s, --postinstall PATH   Postinstall scripts (to run with root credentials at first
                           boot)
  --help                   Show this message and exit.

```

***Note*** - While creating VM with anka create, make sure to specify enough --disk-size.

```
anka create --ram-size 4G --cpu-count 2 --disk-size 80G --app /Applications/Install\ macOS\ High\ Sierra.app build73sierra
Installing macOS 10.13...
Preparing target disk...
Copying addons...
Converting to ANKA format...
Waiting for installation to complete in the guest (about thirty minutes approx.)...
vm created successfully with uuid: 8f0e1111-a14b-11e7-aaa4-003ee1cbb8b4

```
The output of [`anka create`]({{< relref "docs/Anka CLI/command-reference.md#create" >}}) command is a VM created and it's in suspended state. [`anka start`]({{< relref "docs/Anka CLI/command-reference.md#start" >}}) from suspended state bypasses the full boot and starts the Vm in 1-2 seconds.  

***Note*** VMs are created with SIP/Kext Consent disabled by default. It's strongly advised to keep these settings for optimal Anka performance.  

If you need to re-enable SIP/Kext Consent, then use this command `anka modify {template} set custom-variable sys.csr-active-config 0`.

***Note*** VMs created are in suspended mode to enable fast boot/Instant Start.  
### Run VM
The VM can now be successfully started. The VM is preconfigured with a default administrative username `anka` and password `admin`. You will see the VM boot up and have to complete the macOS keypad setup steps.

```
anka start 133b387
+-----------------------+--------------------------------------+
| uuid                  | 49b35a9c-1659-11e8-a71d-003ee1cde439 |
+-----------------------+--------------------------------------+
| name                  | 133b387 (20rel)                      |
+-----------------------+--------------------------------------+
| description           | nineteen                             |
+-----------------------+--------------------------------------+
| created               | Jul 01 07:24                         |
+-----------------------+--------------------------------------+
| cpu_cores             | 4                                    |
+-----------------------+--------------------------------------+
| ram                   | 8G                                   |
+-----------------------+--------------------------------------+
| display               | 1                                    |
+-----------------------+--------------------------------------+
| hard_drive            | 40Gi (43.8Gi on disk)                |
+-----------------------+--------------------------------------+
| addons_version        | 2.0.0.107 (update recommended)       |
+-----------------------+--------------------------------------+
| status                | running                              |
+-----------------------+--------------------------------------+
| vnc_connection_string | vnc://:admin@xxx.xxx.xx.xxx:5901     |
+-----------------------+--------------------------------------+
```

Validate by running the following command `anka run {template} ls -l` from the host. It should display ls -l contents of the host current directory. The VM is correctly created.
You can manually work within the VM with `anka view sierravm`. This will open the VM window.

Do `anka show {template}` to view IP and other runtime details of the VM.

```
anka show 133b387
+-----------------------+--------------------------------------+
| uuid                  | 49b35a9c-1659-11e8-a71d-003ee1cde439 |
+-----------------------+--------------------------------------+
| name                  | 133b387 (20rel)                      |
+-----------------------+--------------------------------------+
| description           | nineteen                             |
+-----------------------+--------------------------------------+
| created               | Jul 01 07:24                         |
+-----------------------+--------------------------------------+
| cpu_cores             | 4                                    |
+-----------------------+--------------------------------------+
| ram                   | 8G                                   |
+-----------------------+--------------------------------------+
| display               | 1                                    |
+-----------------------+--------------------------------------+
| hard_drive            | 40Gi (44.1Gi on disk)                |
+-----------------------+--------------------------------------+
| addons_version        | 2.0.0.107 (update recommended)       |
+-----------------------+--------------------------------------+
| status                | running                              |
+-----------------------+--------------------------------------+
| ip                    | 192.168.64.38                        |
+-----------------------+--------------------------------------+
| vnc_connection_string | vnc://:admin@xxx.xxx.xx.xxx:5901     |
+-----------------------+--------------------------------------+

port_forwarding

+--------------+------------+---------+-------------+-------------+-----------+
|   guest_port | protocol   | name    |   host_port |   time_sync | host_ip   |
+==============+============+=========+=============+=============+===========+
|           22 | tcp        | jenkins |       10000 |           0 | 0.0.0.0   |
+--------------+------------+---------+-------------+-------------+-----------+
```
### Upgrading macOS VM inside Anka VM
***Note*** Don't use the System Preference installer to update macOS inside Anka VM. Use the `softwareupdate -Ri` command line tool to upgrade macOS inside Anka VM. Stop the Vm and then restart it.

### Work inside the VM
There are multiple methods to install various software packages and work inside the VM.

#### Manual method
You can manually work within the VM with `anka view sierravm`. This will open the VM view window.
[`anka view`]({{< relref "docs/Anka CLI/command-reference.md#view" >}}) supports working in full screen and also retina mode. Retina mode is supported for Anka VMs running Mojave or later.

#### Automated methods
**SSH to the VM and execute commands**  

SSH into the VM from the host where its running with the following command.
`ssh anka@ip`, where ip is the Vm IP shown in `anka show {template}` command.

To SSH into the VM from another host, first enable ssh port forwarding. Use [`anka modify`]({{< relref "docs/Anka CLI/command-reference.md#modify" >}}) command.

```
anka modify {template} add port-forwarding --host-port 0 --guest-port 22 ssh
rule added successfully
```
When the port forwarding rule is successfully added, you will see the following in the `anka show {template}` output.

```
port_forwarding

+--------------+------------+---------+-------------+-------------+-----------+
|   guest_port | protocol   | name    |   host_port |   time_sync | host_ip   |
+==============+============+=========+=============+=============+===========+
|           22 | tcp        | ssh     |       10000 |           1 | 0.0.0.0   |
+--------------+------------+---------+-------------+-------------+-----------+

```
Access it with `ssh anka@hotsip -p host_port`

**Use anka run and execute commands**
Similar to `docker exec`, [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) allows execution of commands inside of a VM.

Example
```
anka run -n -N {template} ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
anka run -n {template} sudo gem install xcode-install

#saving user/pass of app store
echo FASTLANE_USER=user >> appstore_login
echo FASTLANE_PASSWORD=password >> appstore_passwd

anka run -f appstore_login -nE {template} xcversion install 10.1
```

Refer here for more details on how to use [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) command.

### Using an ISO file to create Anka VM - Yosemite & ElCapitan VMs

Use `create_macos_install_image.sh` included in the Anka package to first create an `.iso` from your Yosemite and ElCapitan `.app` installer.
If you already have `.iso` file, you don't need to execute this step.
```
/Library/Application\ Support/Veertu/Anka/tools/create_macos_install_image.sh
Usage: create_macos_install_image.sh install_macos.app [OPTIONS]...

Options:
--g,--guest-addons       Embed Anka guest addons in the installer
--o,--output output.iso       Specify output image file, if not specified the image will be created in working directory                  
--p,--pkg path/to/pkg Specify additional packages to include into the installer
```

For example:

```
/Library/Application\ Support/Veertu/Anka/tools/create_macos_install_image.sh /Applications/Install\ macOS\ Sierra.app
```

To create a VM from `.iso`, you will use [`anka create`]({{< relref "docs/Anka CLI/command-reference.md#create" >}}) command as you typically would. It will create an empty VM.
Note - > While creating VM with anka create, make sure to specify enough --disk-size parameter. Currently, it's not possible to change the disk size for an existing VM.

```
anka create --ram-size 2G --cpu-count 2 --disk-size 60G sierravm
vm created successfully with uuid: dfaa97c5-2154-11e8-881d-acbc32ad1d59

```
Then, start the VM with the sierra ISO attached.
```
anka start -v -o sierra.iso sierravm
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

```
anka start -v -o /Library/Application\ Support/Veertu/Anka/guestaddons/anka-addons-mac.iso sierravm
```

Complete the guest addons installation inside the VM. Shutdown the VM with `anka stop {template}`.

Validate by running the following command `anka run {template} ls -l` from the host. It should display ls -l contents of the VM. The VM is correctly created.

Anka Guest Addons also create a default `user - anka`, `passwd - admin` for the VM.




