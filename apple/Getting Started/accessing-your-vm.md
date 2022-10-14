---
title: "Accessing your VM"
linkTitle: "Accessing your VM"
weight: 3
description: >
  Various ways to access your VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "apple/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created and prepared your first VM Template]({{< relref "apple/Getting Started/creating-your-first-vm.md" >}})

## Anka `run`

{{< hint warning >}}
Requires addons are installed inside of the VM. You can check if they are installed with the `anka show {vmName}` command.
{{< /hint >}}

Similar to `docker exec`, [`anka run`]({{< relref "apple/command-line-reference.md#run" >}}) allows execution of commands inside of a VM.

{{< include file="_partials/apple/Anka Virtualization/run/_index.md" >}}

{{< hint info >}}
If the VM is in a _stopped_ state, [`anka run`]({{< relref "apple/command-line-reference.md#run" >}}) will automatically start it.
{{< /hint >}}

You can use [`anka run`]({{< relref "apple/command-line-reference.md#run" >}}) _on the host terminal_ to validate networking inside of the VM:

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

#### Shell Configuration Files / Environment

The [`anka run`]({{< relref "apple/command-line-reference.md#run" >}}) command uses the "default shell" that Apple's API provides inside of macOS and will currently only source `.zprofile`. This is sometimes limited and we recommended that you instead source the files you need or use `zsh/bash -lc or -ic`. Here are some examples:

```shell
anka run 12.6 bash -c "echo 'export TEST_ZSHRC=yes' >> ~/.zshrc"
anka run 12.6 bash -c "echo 'export TEST_ZPROFILE=yes' >> ~/.zprofile"
anka run 12.6 bash -c "echo 'export TEST_PROFILE=yes' >> ~/.profile"
anka run 12.6 bash -c "echo 'export TEST_BASH_PROFILE=yes' >> ~/.bash_profile"

anka run 12.6 env
XPC_SERVICE_NAME=com.veertu.anka.addons.ankarun
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.ozfzOJbvJB/Listeners
PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
XPC_FLAGS=0x0
LOGNAME=anka
USER=anka
HOME=/Users/anka
SHELL=/bin/zsh
TMPDIR=/var/folders/vh/lf52c8657b902x4p2n3splkm0000gn/T/
__CF_USER_TEXT_ENCODING=0x1F5:0x0:0x0
SHLVL=0
OLDPWD=/Users/anka
TEST_ZPROFILE=yes

anka run 12.6 bash -c "env | grep TEST_"
TEST_ZPROFILE=yes

anka run 12.6 bash -ic "env | grep TEST_"
TEST_ZPROFILE=yes

anka run 12.6 bash -lc "env | grep TEST_"
TEST_ZPROFILE=yes
TEST_BASH_PROFILE=yes

anka run 12.6 zsh -c "env | grep TEST_"
TEST_ZPROFILE=yes

anka run 12.6 zsh -ic "env | grep TEST_"
TEST_ZPROFILE=yes
TEST_ZSHRC=yes

anka run 12.6 zsh -lc "env | grep TEST_"
TEST_ZPROFILE=yes
```

{{< hint info >}}
To inherit the host's environment, use the `anka run -E` (existing VM variables will not be overridden by host's variables) or `-e MYENV` options. You can also pass them inside of a file like `anka run --env-file environment.txt`, where environment.txt is a text file in the form `VARIABLE=VALUE`, one variable per line.
{{< /hint >}}


{{< hint info >}}
Some advanced usage examples of `anka run` inside of a bash script can be found in our [Getting Started repo's VM Tag creation script.](https://github.com/veertuinc/getting-started/blob/master/create-vm-template-tags.bash)
{{< /hint >}}

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

## SSH

By default, SSH is enabled inside of the VM. You can SSH into the VM using the VM's IP and port 22:

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

❯ ssh anka@192.168.64.6 -p 22
(anka@192.168.64.6) Password:
Last login: Fri Oct 14 04:25:21 2022
anka@Ankas-Virtual-Machine ~ % 
```

You can also port forward the guest port onto the host so it's accessible from the host IP:

{{< include file="_partials/apple/Anka Virtualization/modify/port/_example.md" >}}

## Anka `view`

### Known Issues

- Chrome, Edge, and any other GPU accelerated browser will not function due to limitations in Apple's hypervisor. You would need to launch the browsers without GPU acceleration. For example, with Chrome: `/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --disable-gpu`.
- `anka view` will not work unless you first started the VM with `anka start -v`. We enable VNC for the VM by default and recommend this approach instead.

### With the CLI

{{< hint warning >}}
The anka viewer requires an active UI session on the host (VNC is fine).
{{< /hint >}}

{{< include file="_partials/apple/Anka Virtualization/view/_index.md" >}}

### With the App

Instead of launching the viewer with the CLI, you can open the Anka.app under /Applications and then double click on the VM in the list. This will launch the viewer window.

---

## Answers to Frequently Asked Questions

- [`anka run`]({{< relref "apple/command-line-reference.md#run" >}}) doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool:

  ```shell
  ❯ anka run VNMANE whoami > /dev/null

  ❯ cat file-on-host.txt | anka run 12.6 md5
  ff1a596f13d348b63218078c6598ab5e
  ```

- You can launch access macOS' Recovery Mode through the Anka.app menu.
  ![recovery-mode]({{< siteurl >}}images/apple/getting-started/accessing-your-vm/anka-app-recovery-mode.png)

## What's next?

- [Modifying your VM]({{< relref "apple/Getting Started/modifying-your-vm.md" >}})
