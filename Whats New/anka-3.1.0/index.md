---
date: 2022-10-13T01:00:00-00:00
title: "Anka Virtualization 3.1.0 (apple/arm64)"
---

We are very excited to announce Anka Virtualization 3.1. In this version, you're going to find several important features that all of our users will benefit from. Here is a summary:

1. [MacOS installation through the `anka create` command is now automated.](#macos-installation-through-the-anka-create-command-is-now-automated)
    - SIP is now disabled by default inside of the VM.
    - VNC is now enabled by default inside of the VM.
2. The `anka create` command will now produce `stopped` state VMs. (suspending is not supported by Apple yet)
3. [Shorter `anka modify` and `port-forwarding` command.](#shorted-anka-modify-and-port-forwarding-command)
4. `ANKA_LOG_LEVEL="debug"` is available as a replacement for `anka --debug`.
5. [Support for the Anka Build Cloud's UAKs when interacting with the registry commands.](#support-for-the-anka-build-clouds-uaks-when-interacting-with-the-registry-commands)
6. [Ability to resize the VM's disk.](#ability-to-resize-the-vms-disk)
7. [Targeting of specific macOS versions with `anka create -a`](#targeting-of-specific-macos-versions-with-anka-create--a)

---

## MacOS installation through the `anka create` command is now automated

Anka 3.0.x began Anka's support for Apple Silicon/M1 virtualization. However, it was significantly different from the Anka 2/Intel version which produced a VM, with `anka create`, with macOS installed, the user set up, and SIP disabled. Users had to manually install macOS and perform any other necessary steps before their VM was functional.

Starting in Anka 3.1, we're the first to solve this problem by automating the macOS installation, setup, and disabling of SIP. We even automatically enable VNC so users can work around the limitations in Anka View (doesn't work under `sudo`). We're excited about this release and look forward to it improving your production environments running Apple Silicon VMs.


## Shorter `anka-modify` and `port-forwarding` command


```bash
❯ anka modify 12.6 add port-forwarding
no argument: name
usage: port-forwarding,port [options] name [rule]

   Add port forwarding rule

arguments:
  name                     Rule name
  rule                     Port forwarding rule: guest-port[:host-ip][:host-port]
. . .

❯ anka modify 12.6 add port-forwarding test 5900:0.0.0.0:50000

❯ anka show 12.6 network
. . .
port_forwarding_rules:
+------+----------+------------+-----------+
| name | protocol | guest_port | host_port |
+------+----------+------------+-----------+
| test | tcp      | 5900       | 50000     |
+------+----------+------------+-----------+
```

## Support for the Anka Build Cloud's UAKs when interacting with the registry commands

[Read more about the Anka Build Cloud Advanced Security Token Feature here.]({{< relref "Anka Build Cloud/Advanced Security Features/token-authentication.md" >}})

{{< include file="_partials/apple/Anka Virtualization/registry/_index.md" >}}

## Ability to resize the VM's disk
```bash
❯ anka show 12.6 disk size
128GiB

❯ anka modify 12.6 disk -s 200G

❯ anka run 12.6 bash -c "df -h"
Filesystem       Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk2s1s1  123Gi   14Gi  106Gi    12%  502068 1106561040    0%   /

❯ anka run 12.6 bash -c "diskutil apfs resizeContainer disk0s2 0"
Started APFS operation
Aligning grow delta to 77,309,411,328 bytes and targeting a new physical store size of 208,855,367,680 bytes
Determined the maximum size for the targeted physical store of this APFS Container to be 208,855,370,752 bytes
Resizing APFS Container designated by APFS Container Reference disk2
The specific APFS Physical Store being resized is disk0s2
Verifying storage system
Using live mode
Performing fsck_apfs -n -x -l /dev/disk0s2
Checking the container superblock
. . .
Verifying volume object map space
The volume /dev/rdisk2s6 appears to be OK
Verifying allocated space
The container /dev/disk0s2 appears to be OK
Storage system check exit code is 0
Growing APFS Physical Store disk0s2 from 131,545,956,352 to 208,855,367,680 bytes
Modifying partition map
Growing APFS data structures
Finished APFS operation

❯ anka run 12.6 bash -c "df -h"
Filesystem       Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk2s1s1  195Gi   14Gi  177Gi     8%  502068 1860793720    0%   /

```

## Targeting of specific macOS versions with `anka create -a`

```bash
❯ anka create --list
+---------+--------+
| version | build  |
+---------+--------+
| 12.6    | 21G115 |
+---------+--------+

❯ anka create -a 12.6 12.6-arm64
 14% [||||||||                                                    ] 10:02 ETA

❯ anka create -a latest 12.6-arm64
```