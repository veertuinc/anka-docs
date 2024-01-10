---
title: "Accessing your VM"
linkTitle: "Accessing your VM"
weight: 3
description: >
  Various ways to access your VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package.]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
2. [You've created and prepared your first VM Template.]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})

## Defaults and Expectations

- Inside of the VMs created with `anka create`, you will find `Screen Sharing` and `Remote Login` enabled automatically. In `shared` networking mode (the default), it allows you to access SSH and VNC using the VM's IP directly from the host running the VM. To access the VM VNC and SSH outside of the host, you'll need to [forward the ports from the VM to the host]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md#port-forwarding-from-guest-to-host" >}}) or enable `bridge` networking mode to get an IP from DHCP in your network.
- Anka Addons are installed into your VM and support `anka run`.
- With `anka view`, the clipboard is not supported. We currently recommend VNC instead.

---

## Anka Run

{{< hint warning >}}
Requires addons are installed inside of the VM. You can check if they are installed with the `anka show {vmName}` command.
{{< /hint >}}

With [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}), you can execute commands inside of a VM.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/run/_index.md" >}}

{{< hint info >}}
If the VM is in a _stopped_ state, [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}) will automatically start it.
{{< /hint >}}

For example you can use [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}) _on the host terminal_ to validate networking inside of the VM:

```shell
❯ anka run 12.6 bash -c "hostname && ls -l && ping -c 5 google.com"
12-2-0-arm.local
total 0
drwx------+  3 anka  staff    96 Oct 14 09:35 Desktop
drwx------+  3 anka  staff    96 Oct 14 09:35 Documents
drwx------+  3 anka  staff    96 Oct 14 09:35 Downloads
drwx------@ 74 anka  staff  2368 Oct 19 11:31 Library
drwx------   4 anka  staff   128 Oct 19 11:14 Movies
drwx------+  3 anka  staff    96 Oct 14 09:35 Music
drwx------+  3 anka  staff    96 Oct 14 09:35 Pictures
drwxr-xr-x+  4 anka  staff   128 Oct 14 09:35 Public
PING google.com (142.251.35.174): 56 data bytes
64 bytes from 142.251.35.174: icmp_seq=0 ttl=108 time=10.316 ms
64 bytes from 142.251.35.174: icmp_seq=1 ttl=108 time=10.270 ms
64 bytes from 142.251.35.174: icmp_seq=2 ttl=108 time=10.163 ms
64 bytes from 142.251.35.174: icmp_seq=3 ttl=108 time=10.305 ms
64 bytes from 142.251.35.174: icmp_seq=4 ttl=108 time=10.281 ms

--- google.com ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 10.163/10.267/10.316/0.055 ms
```

{{< hint warning >}}
(SIP enabled users) You may see the anka run command hang when using, for example, `find`. Opening the Anka Viewer or VNCing in will show a user approval dialog box saying "`ankarund` would like access to" a certain path on the VM. This is due to new security policies Apple is enforcing by default. This is obviously a problem for automation, but, fortunately, there is a solution. You'll need to either avoid using commands that recursively look at the file system locations, or, place the files you wish to find under a "resource" folder under `/Users/anka`. Executing find inside of the folder will not trigger the approval dialog box.
{{< /hint >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/run/_example.md" >}}

{{< hint info >}}
Some advanced usage examples of `anka run` inside of a bash script can be found in our [Getting Started repo's VM Tag creation script.](https://github.com/veertuinc/getting-started/blob/master/create-vm-template-tags.bash)
{{< /hint >}}

---

## SSH

By default, SSH is enabled inside of the VM. You can SSH into the VM using the VM's IP and port 22:

```bash
❯ anka show 14.2.1 network
+------------+-------------------+
| mode       | shared            |
+------------+-------------------+
| controller | virtio-net        |
+------------+-------------------+
| ip         | 192.168.64.6      |
+------------+-------------------+
| mac        | ae:86:1c:97:a5:8a |
+------------+-------------------+

❯ ssh anka@$(anka show 14.2.1 network ip)
(anka@192.168.64.6) Password:
Last login: Fri Oct 14 04:25:21 2022
anka@Ankas-Virtual-Machine ~ % 
```

You can also port forward the guest port onto the host so it's accessible from the host IP:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/port/_example.md" >}}

---

## VNC

By default, we enable VNC inside of VMs created with `anka create`. You can VNC into the VM using the VM's IP and port 5900:

```bash
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

❯ open vnc://192.168.64.6:5900
```

{{< hint info >}}
To expose the VM VNC port on the Host IP, [read more about port forwarding here]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md#port-forwarding-from-guest-to-host" >}}). You will need to create a rule named `vnc` with guest port `5900`. **This is especially important for displaying the VNC url in the Anka Build Cloud Controller's Instances page.**
{{< /hint >}}

---

## Anka View

{{< hint warning >}}
**ARM USERS:** We recommend using VNC over `anka start -v` or `anka view` commands. By default VMs come with VNC enabled and it's far more flexible.
{{< /hint >}}

### Known Issues with `anka view`

- Chrome, Edge, and any other GPU accelerated browser will not function due to limitations in Apple's hypervisor. You would need to launch the browsers without GPU acceleration. For example, with Chrome: `/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --disable-gpu`.
- **ARM USERS:** `anka view` will not work unless you first started the VM with `anka start -v`. We enable VNC for the VM by default and recommend this approach instead.

### With the CLI

{{< hint warning >}}
The anka viewer requires an active UI session on the host (VNC is fine).
{{< /hint >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/view/_index.md" >}}

### With the App

Instead of launching the viewer with the CLI, you can open the Anka.app under /Applications and then double click on the VM in the list. This will launch the viewer window. This is will NOT work when running VMs under the root user.

---

## Answers to Frequently Asked Questions

- [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}) doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool:

  ```shell
  ❯ anka run VNMANE whoami > /dev/null

  ❯ cat file-on-host.txt | anka run 12.6 md5
  ff1a596f13d348b63218078c6598ab5e
  ```

- You can access macOS' Recovery Mode through the Anka.app menu or with `ANKA_START_MODE=2 anka start 12.6`.

  ![recovery-mode]({{< siteurl >}}images/apple/getting-started/accessing-your-vm/anka-app-recovery-mode.png)

## What's next?

- [Modifying your VM]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}})
