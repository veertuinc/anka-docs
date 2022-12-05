---
title: "Understanding VM Networking"
linkTitle: "Understanding VM Networking"
weight: 5
date: 2017-01-20
description: >
  Understanding Anka VM networking
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "apple/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created your first VM Template]({{< relref "apple/Getting Started/creating-your-first-vm.md" >}})
3. [You grasp how to modify VM settings (like `network`)]({{< relref "apple/Getting Started/modifying-your-vm.md" >}})

---

## Basics

Primarily, we use Apple's VMNET interface set to "Using DHCP". By default Anka VMs use a `shared` networking configuration with the host. It’s a kind of NAT + DHCP.

```shell
❯ anka show 12.6 network
+------------+------------+
| mode       | shared     |
+------------+------------+
| controller | virtio-net |
+------------+------------+
. . .
```

Every time you start/resume a VM it will be assigned an IP:

```shell
❯ anka show 12.6
+---------+--------------------------------------+
| uuid    | 1948dd37-e8ea-43b3-972f-b91860329eab |
+---------+--------------------------------------+
| name    | 12.6                                 |
+---------+--------------------------------------+
| created | Oct 12 17:14:31 2022                 |
+---------+--------------------------------------+
| vcpu    | 5                                    |
+---------+--------------------------------------+
| ram     | 6G                                   |
+---------+--------------------------------------+
| display | 1024x768 vnc://192.168.64.6:5900     |
+---------+--------------------------------------+
| disk    | 200GiB (17.20GiB on disk)            |
+---------+--------------------------------------+
| addons  | 3.1.0.148.6247878                    |
+---------+--------------------------------------+
| network | shared 192.168.64.6                  |
+---------+--------------------------------------+
| status  | running since Oct 14 10:28:54 2022   |
+---------+--------------------------------------+

❯ anka show 12.6 network
+------------+-------------------+
| mode       | shared            |
+------------+-------------------+
| controller | virtio-net        |
+------------+-------------------+
| ip         | 192.168.64.6      |
+------------+-------------------+
| mac        | ae:86:1c:97:a5:8a |
+------------+-------------------+
```

{{< include file="_partials/apple/Getting Started/_understanding-vm-networking-types.md" >}}

{{< hint warning >}}
If `anka show` does not display an IP, networking has either:
1. Not fully started (give it a few more seconds).
2. Networking cannot start due to some sort of host firewall or policy.
{{< /hint >}}

{{< hint info >}}
Within the VM, you can find an IP assigned for the host which can be used to ssh or transfer files out. To determine which IP is assigned to the host, execute `ipconfig getoption en0 server_identifier` (typically `192.168.64.1` for **shared** network mode and `192.168.128.1` for **host** network mode).
{{< /hint >}}

### MAC Addresses

Anka will dynamically assign MAC addresses to your VM. You can assign custom MAC Addresses with the `anka modify network --mac` option.

{{< hint warning >}}
Be aware that if you clone your VM Template with a specific MAC, both VMs cannot run at the same time
{{< /hint >}}

{{< hint warning >}}
When using bridged networking mode for your VM, dynamic MAC Addresses are not guaranteed to be unique, though, reuse/collision is extremely unlikely. We do our best to prevent this with our randomization logic.
{{< /hint >}}

---

## FAQs

- Should your Firewall software be blocking VM networking, you need to whitelist the `/Library/Application\ Support/Veertu/Anka/bin/headless.app` (3.x), `/Library/Application\ Support/Veertu/Anka/bin/ankahv.app`, and `/Applications/Anka.app`.

## What's next?

- [Anka Command-Line Reference]({{< relref "apple/command-line-reference.md" >}})
