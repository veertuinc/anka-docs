---
title: "Understanding VM Networking"
linkTitle: "Understanding VM Networking"
weight: 5
date: 2017-01-20
description: >
  Understanding Anka VM networking
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
2. [You've created your first VM Template]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})
3. [You grasp how to modify VM settings (like `network`)]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}})

---

## The Basics

By default Anka VMs use a `shared` networking configuration with the host. This uses a combination of NAT with a local DHCP server provided by macOS, but adds a custom layer that we have more control over.

### Checking network configuration for VMs

#### Stopped VM

```shell
❯ anka show 12.6 network
+------------+------------+
| mode       | shared     |
+------------+------------+
| controller | virtio-net |
+------------+------------+
```

#### Running VM

Every time you start/resume a VM it will be assigned an IP (may take a few seconds for the VM to boot and assign):

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

### Types of networking available

{{< hint info >}}
These are set using [`anka modify`]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}}). Please review the previous section to understand how modifying a VM works.
{{< /hint >}}

{{< include file="_partials/anka-virtualization-cli/getting-started/_understanding-vm-networking-types.md" >}}

{{< hint warning >}}
If `anka show` does not display an IP, networking has either:
1. Not fully started (give it a few more seconds).
2. Networking cannot start due to some sort of host firewall or policy.
{{< /hint >}}

{{< hint info >}}
Within the VM, you can find an IP assigned for the host which can be used to ssh or transfer files out. To determine which IP is assigned to the host, execute `ipconfig getoption en0 server_identifier` (typically `192.168.64.1` for **shared** network mode and `192.168.128.1` for **host** network mode).
{{< /hint >}}

---

### MAC Addresses

Anka will dynamically assign MAC addresses to your VM. You can assign custom MAC Addresses with the `anka modify network --mac` option.

{{< hint warning >}}
Be aware that if you clone your VM Template with a specific MAC, both VMs cannot run at the same time.
{{< /hint >}}

{{< hint warning >}}
You must use a static MAC for each VM running on the machine. You cannot use a mix of dynamically assigned and statically assigned.
{{< /hint >}}


{{< hint warning >}}
When using bridged networking mode for your VM, dynamic MAC Addresses are not guaranteed to be unique, though, reuse/collision is extremely unlikely. We do our best to prevent this with our randomization logic.
{{< /hint >}}

### Default NAT Subnet

VMs are created using the default NAT subnet which can be found with `sudo defaults read /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist Shared_Net_Address`.

To change this, you can use `sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist Shared_Net_Address -string 192.168.80.1`. Changing the `Shared_Net_Mask` is also available with the same modification to the plist.

### DHCP Lease Time

MacOS sets the DHCP timeout to 86,400 seconds (one day) by default. We reuse these leases, which means you will not run out after ~253 VMs in a day. From our testing, Anka's VM networking is much more stable because of this, and not subject to sudden network reconnections and failed tests when the leases timeout. You can check the amount of leases available with `cat /var/db/dhcpd_leases`.

<!-- In order to change this default TTL, you can use `sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.InternetSharing.default.plist bootpd -dict DHCPLeaseTimeSecs -int 1200` (1200 = 20 minutes). -->

---

## Security

We find that many users are interested in VM to VM isolation, VM to Host isolation, and ARP Spoofing prevention. Most macOS virtualization tools on the market do not support network security outside of the defaults Apple provides. We've included features to protect from all three in Anka for both Intel and ARM/Silicon.

### IP Filtering

{{< include file="_partials/anka-virtualization-cli/advanced-security-features/ip-filtering.md" >}}


### VM to VM isolation

This requires using IP Filtering features available for `shared` networking mode. To prevent VM to VM communication, you will use `block local`.

### VM to Host isolation

This requires using IP Filtering features available for `shared` networking mode. To prevent VM to Host communication, you will use `block local`.

### ARP Spoofing Prevention

In order to prevent ARP Spoofing, you'll need to modify the VM with `network --no-local`. This will also prevent VM to VM and VM to Host communication by default. This is also available globally with `sudo anka config no_local 1`.

{{< hint info >}}
Note: `nat` mode does not support VM to Host isolation or ARP Spoofing prevention, even if `no-local` this is enabled.
{{< /hint >}}

---

## FAQs

- Should your Firewall software be blocking VM networking, you need to whitelist the `/Library/Application\ Support/Veertu/Anka/bin/headless.app` (3.x), `/Library/Application\ Support/Veertu/Anka/bin/ankahv.app`, and `/Applications/Anka.app`.

## What's next?

- [Activating your Anka License]({{< relref "anka-virtualization-cli/getting-started/activating-your-anka-license.md" >}})
- [Anka Command-Line Reference]({{< relref "anka-virtualization-cli/command-line-reference.md" >}})
