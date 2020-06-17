---
title: "VM Networking"
linkTitle: "VM Networking"
weight: 5
date: 2017-01-20
description: >
  Understanding Anka VM networking
---

Anka VMs, by default, use a shared networking configuration with the host. It’s a kind of NAT + DHCP. Every time you start/resume a VM, it gets assigned an IP that you can see with `sudo anka show {template}` command.

SSH accesss ("Remote Login") is enabled by by default on Anka VMs, allowing you to:

```shell
ssh anka@<VM IP>
```

> Anka VMs are created with the default user `anka` and a password of `admin`.

In order to access this VM from outside of the host it’s running, [enable port forwarding]({{< relref "docs/Anka Build Cloud/Virtualization CLI/command-reference.md#example---add-port-forwarding" >}}).

## Changing the network configuration for Anka VMs

```shell
sudo anka modify set network-card [OPTIONS] INDEX
```

Options are:

{{< include file="shared/content/en/docs/Anka Build Cloud/Virtualization CLI/partials/modify/set/network-card/_index.md" >}}

> [INDEX] is always 0 since Anka doesn’t support more than 1 NIC per VM.

### Shared type
It is the default network type operating as NAT + DHCP. Every VM after the start/resume gets an IP address assigned by the internal DHCP server in range 192.168.64.2 - 192.168.64.254. 

In order to access the host from inside the VM use the IP address of 192.168.64.1 (or 192.168.128.1 in host-only mode). Programs inside a VM can access external networks (outside the host) and the Internet directly. Also, other VMs on the host are also accessible. Software services can not be accessed directly from external networks. To access such services port forwarding should be configured on the host.

### Host type
It is very similar to the shared one, but VMs get IP addresses from range 192.168.128.2 - 192.168.128.254 and can’t access external networks outside of the host.

### Bridge Type

```shell
sudo anka modify VM set network-card 0 -t bridge
```

Use this to configure bridged networking mode for Anka VMs. VM can be bridged to Ethernet interfaces only (including USB dongles). WiFi interfaces are not supported yet. The interface to bridge can be assigned automatically (default) or specified in VM config.

```shell
sudo anka modify VM set network-card 0 -t bridge -b en0
```

Or default interface can be specified on per-user basis:

```shell
sudo anka config bridge_name en0
```

Since anka settings can be overwritten with env variables, the following will work too:

`ANKA_BRIDGE_NAME=en0 anka start VM …`

VMs with this network type is visible in the network as independent nodes. VMs can be accessed from external networks directly. Since [`sudo anka create`]({{< relref "docs/Anka Build Cloud/Virtualization CLI/command-reference.md#create" >}}) process leaves guest macOS in “Using DHCP” mode, external DHCP server could be needed to get network up immediately, or manual IP address assignment will be needed.

### Disconnected type
With this type of networking the VMs see only “disconnected” ethernet cable. No network communications is available. This mode is pretty much like removal of entire NIC from a VM’s configuration.

```shell
sudo anka modify VM delete network-card 0
```

### MAC addresses
Current Anka implementation uses dynamically assigned MAC addresses. These addresses are being allocated from a pool and can’t be specified directly in VM’s config or so, regardless of the `--mac` option in the `modify` command. The MAC addresses could be reused between different VMs during a time, so VMs can’t be identified from the network by MAC address.

There is the ability to assign static internal MAC address (visible inside VM only) with the `--mac` option. This feature is used if it’s needed to make different VMs, e.g. clones of the same VM, to act as the same network node, but in this case, there could be collisions when several instances of such VMs run on the same host. Static MAC address is being translated to dynamic one and vice-versa during a packet routing.

### Static IP
Anka CLI version 2.2 and greater supports Static IP on VMs running on Catalina hosts (the guest OS can be any supported macOS version). To use static IP addresses follow the following steps:

![Static IP VM](/images/anka-vm-networking/anka-vm-networking.png)

If the network-card setting on the VM is set to `bridge`, the IP address setting is driven by external network configuration.





