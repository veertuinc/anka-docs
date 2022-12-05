---
title: "Understanding VM Networking"
linkTitle: "Understanding VM Networking"
weight: 5
date: 2017-01-20
description: >
  Understanding Anka VM networking
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've got an active license]({{< relref "Licensing/_index.md" >}})
3. [You've created your first VM Template]({{< relref "intel/Getting Started/creating-your-first-vm.md" >}})
4. [You understand how to start and access your VM]({{< relref "intel/Getting Started/starting-and-accessing-your-vm.md" >}})
5. [You grasp how to modify VM settings, like network-card]({{< relref "intel/Getting Started/modifying-your-vm.md" >}})

---

By default, we use Apple's VMNET interface for networking. We also configure the network service as "Using DHCP".

Anka VMs, by default, use a shared networking configuration with the host. It’s a kind of NAT + DHCP. Every time you start/resume a VM, it gets assigned an IP that you can see with `sudo anka show {vmNameOrUUID}` command.

> SSH access ("Remote Login") is enabled by default on Anka VMs.

> Within the VM, you can find an IP assigned to the host which can be used to ssh or transfer files out. To determine which IP is assigned to the host, execute `ipconfig getoption en0 server_identifier` (typically `192.168.64.1` for **shared** network mode and `192.168.128.1` for **host** network mode)

## Changing the network configuration for Anka VMs

{{< include file="_partials/intel/Anka Virtualization/modify/set/network-card/_index.md" >}}

{{< include file="_partials/intel/Anka Virtualization/modify/set/network-card/_example.md" >}}

| Type | Description |
| --- | --- |
| `shared` | The default network type operating as NAT + DHCP. Every VM after the start/resume gets an IP address assigned by the internal DHCP server in range `192.168.64.2 - 192.168.64.254`. Programs inside a VM can access external networks (outside the host) and the internet directly. Also, other VMs on the host are also accessible. {{< rawhtml >}}<br /><br /><blockquote><p>To access ports/services on VMs using a shared network, enable [port forwarding]({{< relref "intel/command-line-reference.md#modify-templatename-add-port-forwarding" >}}) and connect to the host IP at the forwarded port.</p></blockquote>{{< /rawhtml >}} |
| `host` | It is very similar to the shared one, but the VM get IP addresses from range `192.168.128.2 - 192.168.128.254` and can’t access external networks outside of the host. |
| `bridge` | The Bridged type will cause the VM to show in the network as an individual device and receive a unique IP separate from the host. {{< rawhtml >}}<br /><br /><blockquote><p>An ENV is available to set the interface name: `ANKA_BRIDGE_NAME=en0`</p></blockquote><blockquote><p>When using the bridge, port-forwarding is not necessary as the VM will receive a unique IP that will be accessible directly to all other devices on the network.</p></blockquote><blockquote><p>By default, DHCP will not see your VM's MAC address. You'll need to enable `--direct-mac` for the network-card: `anka modify VmName set network-card 0 --type bridge --direct-mac`</p></blockquote>{{< /rawhtml >}} |
| `disconnected` | The VM will have a disconnected network cable. |

### Assigning a Static IP

Anka CLI version 2.2 and greater supports Static IP on VMs running on Catalina hosts (the guest OS can be any supported macOS version):

![Static IP VM]({{< siteurl >}}images/anka-vm-networking/anka-vm-networking.png)

### MAC Addresses

Anka will dynamically assign MAC addresses to your VM. In Catalina and higher, you can assign custom MAC Addresses with the `anka modify set network-card --mac` option.

{{< hint info >}}
Be aware that if you clone your VM Template with a specific MAC, both VMs cannot run at the same time
{{< /hint >}}

{{< hint warning >}}
When using bridged networking mode for your VM, dynamic MAC Addresses are not guaranteed to be unique, though, reuse/collision is extremely unlikely. We do our best to prevent this with our randomization logic.
{{< /hint >}}

{{< hint warning >}}
**In Mojave or older macOS versions:** Even if a custom MAC is set, Apple's VMNET doesn't recognize it on the network outside of the VM. We do a translation from the custom MAC to the one VMNET assigns. This causes the external to VM network to see the VMNET MAC, but inside of the VM you'll see the custom MAC.
{{< /hint >}}

{{< hint info >}}
If a MAC address isn't specified in the VM config, it treated as dynamic and Anka handles such cases for suspended VMs.
{{< /hint >}}

---

## What's next?

- [Activating your Anka License]({{< relref "intel/Getting Started/activating-your-anka-license.md" >}})

## FAQs

- Should your Firewall software be blocking VM networking, you need to whitelist the `ankanetd` process.
