---
date: 2019-01-27
title: "Intel What's New (deprecated)"
weight: 98
description: >
  Description of new Anka software features
---

{{< hint warning >}}
### This page has been deprecated. Please view [the new What's New page for newer releases.]({{< relref "whats-new" >}})
{{< /hint >}}

## What's New in Anka Build Cloud Controller & Registry Version 1.18.0

### Ability to use certs and username/password for etcd connections

In previous version of Anka Build Cloud Controller & Registry it was not possible to connect to an external etcd cluster using certificates or a username and password. We have added several ENVs for you to achieve both of these:

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: | 
Enable Etcd Authentication| bool | Use TLS certificates for authentication with etcd server. **Must pass this for etcd authentication to work** | false | ANKA_USE_ETCD_TLS
Etcd CA Cert| string | Path to CA certificate to be used when connecting to Etcd server | - | ANKA_ETCD_CA_CERT
Etcd Client Cert| string | Path to Etcd Client certificate to be used when connecting to Etcd server | - | ANKA_ETCD_CERT
Etcd Client Key| string | Path to Etcd Client Key to be used when connecting to Etcd server | - | ANKA_ETCD_CERT_KEY
Skip Etcd TLS verification | bool | Don't use TLS verification for Etcd Authentication | false | ANKA_SKIP_ETCD_TLS_VERIFICATION
Enable Etcd user login | bool | Enable Etcd user login when connecting to Etcd server | false | ANKA_USE_ETCD_LOGIN
Etcd Username | string | Etcd username to be used to login to Etcd server | - | ANKA_ETCD_USERNAME
Etcd Password | string | Etcd password to be used to login to Etcd server | - | ANKA_ETCD_PASSWORD

## What's new in Anka Virtualization 2.5.0

### Previous tag deletion method has been replaced

Previously you had to issue `anka delete {template}:{tag}` in order to remove a tag locally (for example if you needed to re-push it to the registry). This has been been replaced with `anka delete {template} --tag {tag}`.

### Multi-stage `anka create` for MDM profile application

Some users find that they need to set the `hw.serial` for a VM before they start the installation of macOS within a VM. We now allow users to issue `anka create {VMNAME}` first, then `anka modify` to set the `hw.serial`, and then continue with the creation using `anka create -a {PATH TO INSTALLER} {VMNAME}. . .`.

### Ability to control VM display frame rate

```bash
Veertu:~ root# anka modify 11.5.2 set display --help
Usage: anka modify set display [OPTIONS]

  Configure displays

Options:
 . . .
  -f, --fps INTEGER           Set refresh rate (30fps by default)
 . . .
```

### Additional `anka show` commands

1. Ability to display information about specific Template Tag:

    ```bash
    ❯ sudo anka show 11.5.2 tags
    +----------------------------------+----------------------+----------------------+
    | tag                              | creation_date        | last_access          |
    +----------------------------------+----------------------+----------------------+
    | vanilla+port-forward-22+brew-git | Aug 18 16:40:28 2021 | Aug 18 17:11:12 2021 |
    +----------------------------------+----------------------+----------------------+
    | vanilla                          | Aug 18 16:21:15 2021 | Aug 18 17:11:12 2021 |
    +----------------------------------+----------------------+----------------------+
    | vanilla+port-forward-22          | Aug 18 16:24:35 2021 | Aug 18 17:11:12 2021 |
    +----------------------------------+----------------------+----------------------+


    ❯ sudo anka show 11.5.2 --tag vanilla+port-forward-22
    +---------+--------------------------------------+
    | uuid    | c12ccfa5-8757-411e-9505-128190e9854e |
    +---------+--------------------------------------+
    | name    | 11.5.2                               |
    +---------+--------------------------------------+
    | created | Aug 18 16:24:35 2021                 |
    +---------+--------------------------------------+
    | vcpu    | 4                                    |
    +---------+--------------------------------------+
    | memory  | 8G                                   |
    +---------+--------------------------------------+
    | display | 1024x768                             |
    +---------+--------------------------------------+
    | disk    | 100GiB (17.31GiB on disk)            |
    +---------+--------------------------------------+
    | addons  | 2.5.0.131 10                         |
    +---------+--------------------------------------+
    | network | shared                               |
    +---------+--------------------------------------+
    | status  | stopped Aug 18 16:24:35 2021         |
    +---------+--------------------------------------+
    ```

1. Display verbose disk information for VM

    ```bash
    Veertu:~ root# anka show 11.5.2 disk
    +--------------+--------------------------------------+
    | controller   | virtio-blk                           |
    +--------------+--------------------------------------+
    | file         | 635c27511ed4453798ab40dcccebe0f0.ank |
    +--------------+--------------------------------------+
    | pci_slot     | 4                                    |
    +--------------+--------------------------------------+
    | image_format | anka                                 |
    +--------------+--------------------------------------+
    | image_size   | 100GiB                               |
    +--------------+--------------------------------------+
    | block_size   | 1MiB                                 |
    +--------------+--------------------------------------+
    | data_size    | 26.84GiB                             |
    +--------------+--------------------------------------+
    | snapshot     | No                                   |
    +--------------+--------------------------------------+
    | encrypted    | No                                   |
    +--------------+--------------------------------------+
    ```

2. Display verbose network information for VM

    ```bash
    Veertu:~ root# anka show 11.5.2 network
    +----------+-------------------+
    | mode     | shared            |
    +----------+-------------------+
    | pci_slot | 28                |
    +----------+-------------------+
    | type     | virtio-net        |
    +----------+-------------------+
    | ip       | 192.168.64.59     |
    +----------+-------------------+
    | mac      | ce:de:e3:e8:44:75 |
    +----------+-------------------+
    | hostif   | en7               |
    +----------+-------------------+

    port_forwarding_rules:
    ```

    > You may notice that `hostif` is set to `en7`. This is the interface on the host the VM is using and it's useful when you want to scan and watch packets with tools like `tcpdump` on the host.

3. Display local tags for VM

    ```bash
    Veertu:~ root# anka show 11.5.2 tags
    +-------------------------+----------------------+----------------------+
    | tag                     | creation_date        | last_access          |
    +-------------------------+----------------------+----------------------+
    | vanilla                 | Aug 18 16:21:15 2021 | Aug 18 16:24:34 2021 |
    +-------------------------+----------------------+----------------------+
    | vanilla+port-forward-22 | Aug 18 16:24:35 2021 | Aug 18 16:24:34 2021 |
    +-------------------------+----------------------+----------------------+
    ```


## What's new in Anka Virtualization 2.4.0

### VM networking is now isolated for improved security

VMs can now be isolated from access to other VMs or even then host itself using `sudo anka modify 11.2.3 set network-card --no-local`.

{{< include file="_partials/intel/Anka Virtualization/modify/set/network-card/_index.md" >}}
### Registry pushing and pulling of VM Templates/Tags are now chunked for better performance

Pushing and pulling templates/tags that are large can be impacted by network interrupts or limits. We've added the ability for you to set the chunk size using `anka config chunk_size {bytes}` on your nodes to solve this.

> `chunk_size` must be set BEFORE VM creation

> `chunk_size` of 0 is unlimited (the entire layer is uploaded at once)

```shell
❯ anka config chunk_size
0

# Set 2GB chunk size
❯ anka config chunk_size 2147483648
```

> We'll parallelize the push or pull chunks to the number set in `anka config puller_threads` (puller_threads is also used for push threads; there is no push_threads)

## What's new in Anka Virtualization 2.3.4

### Expose the bridged VM's MAC address on the host so that DHCP can assign properly

> Requires bridged networking 

```bash
anka modify VmName set network-card 0 --type bridged --direct-mac
```

## What's New in Jenkins Plugin 2.4.0

### Set various VM Launch timeout values

![Launch Values Configurable 2.4.0]({{< siteurl >}}images/whatsnew/jenkins-2.4.0-launch-values.png)
## What's New in Anka Build Cloud Controller & Registry Version 1.14.0

### Delete button will show for Offline Nodes

![Controller Delete Node Button]({{< siteurl >}}images/whatsnew/controller-1.14.0-delete-node-button.png)
### Customize the range of MAC addresses the controller uses for creating VMs

You can now specify the range of MAC addresses that the controller uses with the `ANKA_MANAGE_MAC_ADDRESSES` config.

Once configured, [you can use the API to list or delete/regenerate the MAC list from a new range]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#mac-addresses-management" >}}).

> Requires that you enable `ANKA_MANAGE_MAC_ADDRESSES`
## What's new in Anka Virtualization 2.3.3

### You can now collect your license Fulfillment IDs before you remove the license

Failing hardware is rare, but when it happens it leaves customers without the ability to run `anka license remove` and send us to Fulfillment ID to detatch the used cores.

Starting in 2.3.3 you can now run `anka license show` and see the ID. We recommend storing these IDs in your internal documentation.

```bash 
anka license show
+---------------------+------------------------------+
| license_type        | com.veertu.anka.entplus.host |
+---------------------+------------------------------+
| status              | valid                        |
+---------------------+------------------------------+
| expires             | 31-dec-2021                  |
+---------------------+------------------------------+
| max_number_of_cores | 32                           |
+---------------------+------------------------------+
| fulfillment         | 36094XXXXX                   |
+---------------------+------------------------------+
```

## What's new in Anka Virtualization 2.3.2

### PG ("Metal") support inside of the VM

> A Big Sur Host and VM is required

Apple has included Paravirtualized Graphics support for VMs in Big Sur, allowing us to support graphics acceleration for Anka customers.

To enable this feature, you simply need to:

1. Upgrade the host to Big Sur
2. Create a Big Sur VM
3. Set the display: `anka modify {vmName} set display -c pg`

```bash
❯ anka modify 11.1.0 set display --help
Usage: anka modify set display [OPTIONS]

  Configure displays

Options:
  -n, --count INTEGER         Configure number of displays (2 max)
  --headless                  same as --count 0
  -p, --password              Prompt for VNC password
  -v, --vnc TEXT              Configure VNC
  --no-vnc                    Disable VNC access to the VM
  -d, --dpi INTEGER           Set DPI (default is 72)
  -r, --resolution TEXT       Set resolution, e.g. 1024x768 or 'default' to reset to default
  -c, --controller [fbuf|pg]  Set video controller
  --host-gpu-location TEXT    Specify location of the host GPU to use, or 'any'
  --host-gpu-name TEXT        Specify name of the host GPU to use, or 'any'
  --features INTEGER          Specify extended features flags as a bit mask
  --help                      Display usage information
```

> At the moment, the `pg` video controller only works for stopped VMs

## What's New in Anka Build Cloud Controller & Registry Version 1.13.0

### `ankacluster status` now shows more details about the Node

```bash
❯ sudo ankacluster status
Password:
status: running
config:
  vm_limit: 2
  optimization_threshold: 5
  num_workers: 2
  controller_addresses:
  - http://anka.controller
  version: 1.13.0-24e848a5
  capacity_mode: number
  heartbeat: 5s
  node_name: Veertu.fios-router.home
  vm_stuck_check_delay: 30s
  vm_stuck_check_timeout: 10s


❯ sudo ankacluster status --machine-readable | jq
{
  "status": "running",
  "config": {
    "vm_limit": 2,
    "optimization_threshold": 5,
    "tls_certs": {
      "UseTLS": false,
      "RootCert": "",
      "ClientKeyStore": "",
      "KeyStorePass": "",
      "ClientCert": "",
      "ClientCertKey": "",
      "CACert": "",
      "SkipTLSVerification": false
    },
    "num_workers": 2,
    "controller_addresses": [
      "http://anka.controller"
    ],
    "version": "1.13.0-24e848a5",
    "capacity_mode": "number",
    "heartbeat": 5000000000,
    "node_name": "Veertu.fios-router.home",
    "vm_stuck_check_delay": 30000000000,
    "vm_stuck_check_timeout": 10000000000
  }
}
```

## What's new in Anka Virtualization 2.3.1

### You can now untag a VM

On pushing to the registry, a tag is created. It will also be assigned a specific commit ID (not visible to users). Even if you modify the tag locally, such as adding port-forwarding, changes will not be pushed to the registry until you push with a different tag name.

Now, you can simply untag the VM locally and then push it with the same name (after [deleting the VM template]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#delete-template" >}}) or [reverting the tag]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#revert-template-tag" >}})):

> Locally, this does not remove the current STATE of the tag from the VM. Your installed dependencies inside of the VM will remain as long as you don't pull or switch to a different tag.

```bash
anka registry pull -t tag2 VM
anka delete VM:tag2
anka modify VM add port-forwarding...
curl ... (delete or revert from registry)
anka registry push -t tag2 VM
```

## What's New in Packer Plugin 1.6.0

### Base VM and Clone default name changes

To remove a lengthy part of iteration when starting from an installer app, base VMs are now created with the stable name of `anka-packer-base-{macOSVersion}` and cloned VMs with `anka-packer-{10RandomChars}`.

### Ability to set port-forwarding

```json
{
  "variables": {
    "source_vm_name": ""
  },
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "sleep 5",
        "echo hello world",
        "echo llamas rock"
      ]
    }
  ],
  "builders": [{
    "type": "veertu-anka",
    "cpu_count": 8,
    "ram_size": "10G",
    "source_vm_name": "{{user `source_vm_name`}}",
    "port_forwarding_rules": [
      {
        "port_forwarding_guest_port": 80,
        "port_forwarding_host_port": 12345,
        "port_forwarding_rule_name": "website"
      },
      {
        "port_forwarding_guest_port": 8080
      }
    ]
  }]
}
```

### Ability to set hw.uuid


```json
{
  "variables": {
    "source_vm_name": "",
    "hw_uuid": "{{env `HW_UUID`}}"
  },
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "sleep 5",
        "echo hello world",
        "echo llamas rock"
      ]
    }
  ],
  "builders": [
    {
      "type": "veertu-anka",
      "hw_uuid": "{{user `hw_uuid`}}",
      "cpu_count": 10,
      "ram_size": "12G",
      "source_vm_name": "{{user `source_vm_name`}}"
    }
  ]
}
```


### Modifying disk_size now automatically resizes the internal disk by executing `diskutil`

```bash
==> veertu-anka: Modifying VM anka-packer-UUFnqCbIGf disk size to 150G
2020/12/01 12:58:33 packer-builder-veertu-anka plugin: 2020/12/01 12:58:33 Executing anka --machine-readable modify anka-packer-UUFnqCbIGf set hard-drive -s 150G
2020/12/01 12:59:21 packer-builder-veertu-anka plugin: 2020/12/01 12:59:21 {"status": "OK", "body": {}, "message": ""}
2020/12/01 12:59:21 packer-builder-veertu-anka plugin: 2020/12/01 12:59:21 Starting command: anka run -n anka-packer-UUFnqCbIGf sh
2020/12/01 12:59:21 packer-builder-veertu-anka plugin: 2020/12/01 12:59:21 Executing on sh: diskutil apfs resizeContainer disk1 0
2020/12/01 12:59:21 packer-builder-veertu-anka plugin: 2020/12/01 12:59:21 Waiting for command to run
2020/12/01 13:00:03 packer-builder-veertu-anka plugin: 2020/12/01 13:00:03 Command finished in 42.58007918s with <nil>
```

## What's New in Jenkins Plugin 2.3.0

### Ability to disable timestamp

When using the cache builder to automate your VM template creation in Jenkins, you can now remove the appended timestamp.

**Static Templates (found under `/configureClouds`):** You will see a `Don't append Timestamp` checkbox under the Cache Builder section of your Static Template definition.

**Dynamic Labelling Function (`CreateDynamicAnkaNode`):** You can set `dontAppendTimestamp` to true.

## What's New in Anka Build Cloud Controller & Registry Version 1.12.0

### Control allowed TLS/SSL Protocols and Ciphers

You can now set the allowed Cipher Suites and min/max TLS versions that the controller will accept. This is great if your security teams require forcing certain suites and versions.

`/usr/local/bin/anka-controllerd` example:

```
export ANKA_CIPHER_SUITES="tls_ecdhe_ecdsa_with_chacha20_poly1305_sha256"
export ANKA_MAX_TLS_VERSION="tls_1.0"
```

A full list of options is available in the [configuration reference]({{< relref "Anka Build Cloud/configuration-reference.md#https--tls" >}}).

## What's new in Anka Virtualization 2.3.0

> This release does not make significant changes to macOS Catalina (and older) VMs. They will function the same as prior releases of Anka. We recommend updating addons regardless.

### Use Anka for free!

Our new Anka Develop license is a free, but limited, version of our Anka Virtualization software. You will see the license enabled by default after you install the Anka Virtualization.

Anka Develop is currently restricted to running a single VM at a time. It also does not include VM suspension, tagging, or use of the registry CLI commands. Finally, it will only function on Macbook, Macbook Pro, and Macbook Air hardware.

### VM Management UI

We now have a management UI allowing you to start, stop, delete, and create VMs. This is used as an alternative to the CLI.

![installer with pkg]({{< siteurl >}}images/getting-started/creating-your-first-vm/create-vm-window-with-options.png)

### Big Sur (11.X) support

You can now use `anka create` to create 11.X macOS versions.

### Anka is now driverless!

Starting in Big Sur, we have recreated our driver addons as driverless addons. This allows for much more feature flexibility moving forward. This opens up many opportunities for our developers to add new features.

### New `anka cp` / Deprecated `anka mount` and `anka run` automatic mounting

Due to security changes in Big Sur, we've had to rewrite all of our addons to be driverless. This means that `anka mount` and `anka run (mounting)` no longer work with Big Sur VMs. However, they do still function for Catalina and below.

As an alternative, we now have `anka cp` which allows transferring files into the VM. The transfer speeds have shown to be much faster than SCP.

{{< include file="_partials/intel/Anka Virtualization/cp/_index.md" >}}
{{< include file="_partials/intel/Anka Virtualization/cp/_example.md" >}}

> Be sure to update addons with `anka start -u 10.15.7` to use `anka cp` with Catalina (or older) VMs

### Customize the Hostname inside of your VM on start

You can now automatically set the VM hostname to the VM name on start by enabling `propagate_name`:

```bash
❯ anka config propagate_name True
```

If you'd like to customize the hostname, you can set the `ANKA_HOSTNAME` env first:

```bash
❯ hostname
Node-1.local

❯ ANKA_HOSTNAME="$(hostname)-$RANDOM" anka run 11.0.1 bash -c "echo \$(hostname)"
node-1-10180.local
```

## What's New in Anka Build Cloud Controller & Registry Version 1.10.1

### Unresponsive VM Monitor

Rarely we see certain dependencies installed inside of VMs causing the entire VM to become unresponsive. Unresponsive or "stuck" VMs can run indefinitely and therefore cause CI/CD jobs to run until they hit timeouts. This is a bad experience for users, so we've added a feature which constantly checks whether or not the command-line inside of VM is responding to a simple command. If it cannot, the Anka Agent on your Node will fail the VM, causing the CI/CD job to receive a failure and then send a Termination request to the Controller. 

You can enable this feature when you join your Node to the Anka Cloud with: `sudo ankacluster join --enable-vm-monitor http://anka.controller`.

You can see the flags and options with:

```
❯ ankacluster join --help
Joins the current machine to one or many Anka Build Cloud Controllers

Usage:
  ankacluster join CONTROLLER_ADDRESS[,CONTROLLER_ADDRESS2] [flags]

Flags:
...
  --enable-vm-monitor           Enabled unresponsive VM monitoring. This will throw a failure when the VM becomes unresponsive for longer than the --vm-stuck-timeout
...
  --vm-stuck-delay duration     The time between unresponsive VM checks (default: 30s - Duration examples: 3500s, 20m, 5h)
  --vm-stuck-timeout duration   The time to wait until the VM is considered unresponsive (default: 10s - Duration examples: 3500s, 20m, 5h)
```

## What's New in Anka Build Cloud Controller & Registry Version 1.9.0

### Track MAC Addresses for VMs and prevent rare collision situations

It's extremely rare that MAC addresses assigned to VMs collide. We've added the ability for the Controller to keep track of MAC addresses and prevent reuse. You can also manually specify the MAC address for the VM using the Controller API.

> Your VM Templates and Tags must be in a stopped state in the Registry/on your Nodes to set the MAC address properly.

> If you have VMs already running when you upgrade your Controller, they will not be managed by this feature until they are restarted through the Controller.

## What's New in Jenkins Plugin Version 2.1.0

### Static Slave and Cloud Instance Limits (support for || label operator)

When specifying the label to use in your Jenkins pipelines, you're able to use operators like `||` (OR). Previous versions of our Jenkins plugin had problems with this as each Static Slave Template you configured would be able to use unlimited instances (as many as your controller allowed). In 2.1.0, you have the ability to set a cap on the amount of instances allowed to be created for the specific Static Slave Template or even for the entire Anka Cloud definition. As an example:

- Your pipeline uses `large-fleet-ios || small-fleet-ios`
- You have two Static Slave Templates: 
    1. label `large-fleet-ios`
        - has lots of resources per VM, but far less total VMs that can run
        - has an instance/VM cap of 5
    2. label `small-fleet-ios` that has less resources because more VMs are running at once
        - has an instance/VM cap of 20
- Your large node fleet has a total of 10 VMs that can run at once (5 nodes, 2 VMs per node) (more expensive mac minis with lots of cpu and memory)
- Your small node fleet has a total of 30 VMs that can run at once (15 nodes, 2 VMs per node) (cheaper mac minis with less resources)

When someone runs an iOS build, you want it to use the large-fleet if possible so the builds and tests take less time. With this setup, your pipeline will run and request a VM from the Static Slave Template with `large-fleet-ios` as the label. However, if other pipeline jobs are already running and using all 5 instance slots, it will then fall back onto using `small-fleet-ios`.

Notice that our instance cap for `large-fleet-ios` is 5 and we have 10 total VMs for the large-fleet. This allows other labels for the large fleet the remaining 5 slots at all times if there are lots of `large-fleet-ios` requests. This helps balance the usage between multiple projects.

### Slave/Node name is now available within the 1.8.0 Controller

Changes to the way we communicate with the Controller API now allows for the slave name and slave/jenkins UI URL to be displayed under the custom columns.

![Jenkins 2.1.0 Slave/Node Info Page]({{< siteurl >}}images/whatsnew/jenkins-2.1.0-slave-info-page.png)

![Custom Column Jenkins 2.1.0]({{< siteurl >}}images/whatsnew/jenkins-2.1.0-custom-columns.png)

## What's New in Anka Build Cloud Controller Version 1.8.0

### Set custom instance metadata and show it in the dashboard

![Custom Column]({{< siteurl >}}images/whatsnew/controller-instances-custom-column.png)

Create an instance with metadata:

```bash
❯ curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
-d '{"vmid": "c0847bc9-5d2d-4dbc-ba6a-240f7ff08032", "count": 1, "metadata": { "test": true }}'

{"status":"OK","message":"","body":["a681f959-b4db-4f3b-7a88-0384ad146bf4"]}
```

Then show it as a column in the dashboard:

![Custom Column from Metadata]({{< siteurl >}}images/whatsnew/controller-instances-custom-metadata-column.png)

### Change the vCPU and RAM for a stopped VM Template

```bash
❯ sudo anka show 10.15.4-stopped
+----------------+--------------------------------------+
| uuid           | 3a65b45e-c08d-44e9-84e2-ba45d646af4a |
+----------------+--------------------------------------+
| name           | 10.15.4-stopped (test)               |
+----------------+--------------------------------------+
| created        | 100 seconds ago                      |
+----------------+--------------------------------------+
| cpu_cores      | 6                                    |
+----------------+--------------------------------------+
| ram            | 10G                                  |
+----------------+--------------------------------------+
| display        | 1                                    |
+----------------+--------------------------------------+
| hard_drive     | 80Gi (16.6Gi on disk)                |
+----------------+--------------------------------------+
| addons_version | 2.2.3.118                            |
+----------------+--------------------------------------+
| status         | stopped 29 seconds ago               |
+----------------+--------------------------------------+
```

Create your instance and set the CPU and RAM:

```bash
❯ curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
-d '{"vmid": "3a65b45e-c08d-44e9-84e2-ba45d646af4a", "count": 1, "vcpu": 4, "vram": 8192 }'

{"status":"OK","message":"","body":["ac653ec9-5c7e-4b99-5749-ec0d242ca958"]}
```

This is change the values of the cloned/started VM:

```bash
sh-3.2# anka show mgmtManaged-10.15.4-stopped-1592271338138326000
+-----------------------+-------------------------------------------------+
| uuid                  | e767f994-e366-460a-9dbd-5e2eb9729242            |
+-----------------------+-------------------------------------------------+
| name                  | mgmtManaged-10.15.4-stopped-1592271338138326000 |
+-----------------------+-------------------------------------------------+
| created               | 10 seconds ago                                  |
+-----------------------+-------------------------------------------------+
| cpu_cores             | 4                                               |
+-----------------------+-------------------------------------------------+
| ram                   | 8G                                              |
...
```


## What's New in Version 2.2.3
### Anka 2.2.3 - May 03, 2020

**When using Anka Viewer, provided a way to get out of full screen, after in full screen mode (using the green full screen button on the window's top bar)**

**Updated license terms**

## What's New in Anka Build Cloud Controller Version 1.7.0

**Disk Usage for Node is displayed in the controller Dahsboard and REST API**

![Node Disk Usage Information]({{< siteurl >}}images/whatsnew/Nodestorageinfo.png)

**Size information for Templates and tags is displayed in the controller dashboard and REST API**

![Template Size Information]({{< siteurl >}}images/whatsnew/templatesize.png)

**Jenkins job/Node information is displayed for the VM provisioned in the controller dashboard**

![Jenkins Job Information]({{< siteurl >}}images/whatsnew/JenkinsInfoportal.png)

**New reserve disk flag in ankacluster join command**

When `–reserve-space` flag is set, controller will always reserve the disk space before pulling VM template on the node. If there is not enough disk space after allocating for `–reserve-space`, then controller will not pull the Vm Template. This flag is provided to avoid scenario where there is no disk psace left on the node to accomodate for extra disk usage duing builds inside the VM.

![reservedisk ankacluster flah]({{< siteurl >}}images/whatsnew/ankaclusterreservediskspace.png)

## What's New in Anka Version 2.2.2

**License passthrough from host to nested enabled Anka VM**

This feature enables you to install Anka binary package inside an Anka macOS VM and use it create and run Anka VMs.

1. Enable nested for the Anka VM in which you want to install Anka binary.
2. Install Anka binary package inside the VM.
2. Execute `anka config propagate_license 1` on the host machine.
3. Stop and start the nested enabled VM.
4. Check with `sudo anka license show` inside the VM. It will show the host license information.

**Set DPI for the VM**

Added a new flag in `anka modify set display` command to set DPI for the VM.

```
anka modify set display [OPTIONS]
  configure displays
Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  --no-vnc               disable VNC access to the VM
  -d, --dpi INTEGER      set DPI (default is 72)
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
```

**Disable VNC access to the VM**

Added a new flag in `anka modify set display` command to disable VNC access for the VM.

```
anka modify set display [OPTIONS]
  configure displays
Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  --no-vnc               disable VNC access to the VM
  -d, --dpi INTEGER      set DPI (default is 72)
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
```



## What's New in Anka Version 2.2.1

**Set resolution display for the VM with `anka modify set display`.**

```
anka modify set display [OPTIONS]

  configure displays

Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
  ```

  



**Send virtual bound events programmatically. This can be used to automate bypass of Apple confirmation dialog.**

 ```
 anka view [OPTIONS] VM_ID

  Open VM display viewer

Options:
  -d, --display INTEGER  Specify the displays to view
  -s, --screenshot       Make png screenshot
  -c, --click TEXT       Send the pointer event
  --help                 Show this message and exit.
  ```


`anka view VM --click 100,200` will click virtual click event at location 100*200

***Note*** Also available through /Library/Application\ Support/Veertu/Anka/addons/click 100,200

  

