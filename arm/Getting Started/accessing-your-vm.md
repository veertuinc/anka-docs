---
title: "Accessing your VM"
linkTitle: "Accessing your VM"
weight: 3
description: >
  Various ways to access your VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "arm/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created and prepared your first VM Template]({{< relref "arm/Getting Started/creating-your-first-vm.md" >}})

{{< hint warning >}}
**The Apple hypervisor and also Anka commands require an active and logged in user. You might have to VNC in to the host machine if you're connected over SSH. This also means you need to disable any sort of sleep or even the passworded screensaver on macOS.**
{{< /hint >}}

## Anka `run`

{{< hint warning >}}
Requires addons are installed inside of the VM. You can check if they are installed with the `anka show {vmName}` command.
{{< /hint >}}

Similar to `docker exec`, [`anka run`]({{< relref "arm/command-line-reference.md#run" >}}) allows execution of commands inside of a VM.

{{< include file="_partials/arm/Anka Virtualization/run/_index.md" >}}

{{< hint info >}}
If the VM is in a _stopped_ state, [`anka run`]({{< relref "arm/command-line-reference.md#run" >}}) will automatically start it.
{{< /hint >}}

You can use `anka run` _on the host terminal_ to validate things are working properly:

```shell
❯ anka run 12.2.0-arm bash -c "hostname && ls -l && ping -c 5 google.com"
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

#### Shell Configuration Files / Environment

The [`anka run`]({{< relref "arm/command-line-reference.md#run" >}}) command doesn't source `.profile`, `.bash_profile`, or `.zshrc` by default. It will however source `.zprofile`.

You have to source the files or use `zsh/bash -lc/-ic` before executing other commands. Here are some examples:

```shell
❯ anka run 12.2.0-arm bash -c "echo 'export TEST_ZSHRC=yes' >> ~/.zshrc"
❯ anka run 12.2.0-arm bash -c "echo 'export TEST_ZPROFILE=yes' >> ~/.zprofile"
❯ anka run 12.2.0-arm bash -c "echo 'export TEST_PROFILE=yes' >> ~/.profile"
❯ anka run 12.2.0-arm bash -c "echo 'export TEST_BASH_PROFILE=yes' >> ~/.bash_profile"
❯ anka run 12.2.0-arm env
TEST_ZPROFILE=yes
❯ anka run test bash -c "env | grep TEST_"
TEST_ZPROFILE=yes
❯ anka run test bash -ic "env | grep TEST_"
TEST_ZPROFILE=yes
❯ anka run test bash -lc "env | grep TEST_"
TEST_ZPROFILE=yes
TEST_BASH_PROFILE=yes
❯ anka run test zsh -c "env | grep TEST_"
TEST_ZPROFILE=yes
❯ anka run test zsh -ic "env | grep TEST_"
TEST_ZPROFILE=yes
TEST_ZSH=yes
❯ anka run test zsh -lc "env | grep TEST_"
TEST_ZPROFILE=yes
```

{{< hint info >}}
To inherit the host's environment, use the `anka run -E` (existing VM variables will not be overridden by host's variables) or `-e MYENV` options. You can also pass them inside of a file like `anka run --env-file environment.txt`, where environment.txt is a text file in the form `VARIABLE=VALUE`, one variable per line.
{{< /hint >}}


{{< hint info >}}
Some advanced usage examples of `anka run` inside of a bash script can be found in our [Getting Started repo's VM Tag creation script.](https://github.com/veertuinc/getting-started/blob/master/create-vm-template-tags.bash)
{{< /hint >}}

## Anka Viewer

### Known Issues

- Chrome, Edge, and any other GPU accelerated browser will not function due to limitations in Apple's hypervisor. You would need to launch the browsers without GPU acceleration. For example, with Chrome: `/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --disable-gpu`.

### With the CLI

{{< hint warning >}}
The anka viewer requires an active UI session on the host (VNC is fine).
{{< /hint >}}

{{< hint warning >}}
The `anka view` command currently will only function if you started the VM with `anka start -uv`.
{{< /hint >}}

{{< include file="_partials/arm/Anka Virtualization/view/_index.md" >}}

### With the App

Instead of launching the viewer with the CLI, you can open the Anka.app under /Applications and then double click on the VM in the list. This will launch the viewer window.


## SSH

{{< include file="_partials/arm/Anka Virtualization/modify/add/port/_example.md" >}}

## VNC

Once you've enabled Apple's Remote Login inside of the VM, simply add a forwarded port: `anka modify 12.2.0-jenkins add port --guest-port 5900 vnc`.

---

## Answers to Frequently Asked Questions

- [`anka run`]({{< relref "arm/command-line-reference.md#run" >}}) doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool:

  ```shell
  ❯ anka run VNMANE whoami > /dev/null

  ❯ cat file-on-host.txt | anka run 12.2.0-arm md5
  ff1a596f13d348b63218078c6598ab5e
  ```

- You can launch access macOS' Recovery Mode through the Anka.app menu.
  ![recovery-mode]({{< siteurl >}}images/arm/getting-started/accessing-your-vm/anka-app-recovery-mode.png)

## What's next?

- [Modifying your VM]({{< relref "arm/Getting Started/modifying-your-vm.md" >}})
