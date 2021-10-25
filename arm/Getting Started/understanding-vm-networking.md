---
title: "Understanding VM Networking"
linkTitle: "Understanding VM Networking"
weight: 5
date: 2017-01-20
description: >
  Understanding Anka VM networking
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "arm/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created your first VM Template]({{< relref "arm/Getting Started/creating-your-first-vm.md" >}})
3. [You grasp how to modify VM settings (like `network`)]({{< relref "arm/Getting Started/modifying-your-vm-template.md" >}})

---

We use Apple's VMNET interface with "`Using DHCP`" for networking. 

By default Anka VMs use a shared networking configuration with the host. It’s a kind of NAT + DHCP. 

```shell
❯ anka --machine-readable describe 12.0-beta | jq '.body.network_cards'
[
  {
    "mode": "shared",
    "controller": "virtio-net"
  }
]
```

Every time you start/resume a VM it will be assigned an IP:

```shell
❯ anka show 12.0-beta
+---------+--------------------------------------+
| uuid    | 26c18e20-f67a-4387-a7b7-236a277bb424 |
+---------+--------------------------------------+
| name    | 12.0-beta (v2)                       |
+---------+--------------------------------------+
| created | Oct 18 13:57:57 2021                 |
+---------+--------------------------------------+
| vcpu    | 8                                    |
+---------+--------------------------------------+
| memory  | 12G                                  |
+---------+--------------------------------------+
| display | 1024x768                             |
+---------+--------------------------------------+
| disk    | 128GiB (22.84GiB on disk)            |
+---------+--------------------------------------+
| addons  | 3.0.0.135.8400565                    |
+---------+--------------------------------------+
| network | shared 192.168.64.6                  |
+---------+--------------------------------------+
| status  | running since Oct 25 15:48:36 2021   |
+---------+--------------------------------------+

❯ anka show 12.0-beta network
+------------+-------------------+
| mode       | shared            |
+------------+-------------------+
| controller | virtio-net        |
+------------+-------------------+
| ip         | 192.168.64.6      |
+------------+-------------------+
| mac        | ce:6b:90:1b:87:da |
+------------+-------------------+
```

{{< hint warning >}}
If `anka show` does not display an IP, networking has either:
1. Not fully started (give it a few more seconds).
2. Networking cannot start due to some sort of host firewall or policy.
{{< /hint >}}

{{< hint info >}}
Within the VM, you can find an IP assigned to the host which can be used to ssh or transfer files out. To determine which IP is assigned to the host, execute `ipconfig getoption en0 server_identifier` (typically `192.168.64.1` for **shared** network mode and `192.168.128.1` for **host** network mode).
{{< /hint >}}

## Changing the network configuration for Anka VMs

{{< include file="_partials/arm/Anka Virtualization/modify/network/_index.md" >}}

{{< include file="_partials/arm/Anka Virtualization/modify/network/_example.md" >}}

| Type | Description |
| --- | --- |
| `shared` | The default network type operating as NAT + DHCP. Every VM after the start/resume gets an IP address assigned by the internal DHCP server in range `192.168.64.2 - 192.168.64.254`. Programs inside a VM can access external networks (outside the host) and the internet directly. Also, other VMs on the host are also accessible. |
| `host` | {{< hint warning >}}Not currently supported.{{< /hint >}} |
| `bridge` | The Bridged type will cause the VM to show in the network as an individual device and receive a unique IP separate from the host. {{< hint info >}}An ENV is available to set the interface name: `ANKA_BRIDGE_NAME=en0`{{< /hint >}}{{< hint info >}}When using the bridge, port-forwarding is not necessary as the VM will receive a unique IP that will be accessible directly to all other devices on the network.{{< /hint >}}{{< hint info >}}By default, DHCP will not see your VM's MAC address. You'll need to enable `--direct-mac` for the network-card: `anka modify VmName set network-card 0 --type bridged --direct-mac`{{< /hint >}} {{< hint warning >}}You cannot use bridged networking over Wifi.{{< /hint >}} |
| `disconnected` | The VM will have a disconnected network cable. |

### MAC Addresses

Anka will dynamically assign MAC addresses to your VM. In Catalina and higher, you can assign custom MAC Addresses with the `anka modify network --mac` option.

{{< hint warning >}}
Be aware that if you clone your VM Template with a specific MAC, both VMs cannot run at the same time
{{< /hint >}}

{{< hint warning >}}
Dynamic MAC Addresses are not guaranteed to be unique, though, reuse/collision is rare
{{< /hint >}}

---


## FAQs

- Should your Firewall software be blocking VM networking, you need to whitelist the `ankanetd` process.

## What's next?

- [Anka CLI Command Reference]({{< relref "arm/Anka Virtualization/command-reference.md" >}})
