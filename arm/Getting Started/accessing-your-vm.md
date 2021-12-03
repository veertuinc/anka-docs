---
title: "Accessing your VM"
linkTitle: "Accessing your VM"
weight: 3
description: >
  Various ways to access your VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "intel/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've created and prepared your first VM Template]({{< relref "intel/Getting Started/creating-your-first-vm.md" >}})

{{< hint info >}}
When running Anka commands on your machine, ensure that you have an active login session and have enabled automatic login for the current user: `System Preferences > Users > Enable automatic login` for the host. You might have to VNC in to the host machine if you're connected over SSH.
{{< /hint >}}

## Anka Run (exec)

{{< hint warning >}}
Requires that addons are installed inside of the VM.
{{< /hint >}}

Similar to `docker exec`, [`anka run`]({{< relref "arm/Anka Virtualization/command-reference.md#run" >}}) allows execution of commands inside of a VM.

{{< include file="_partials/arm/Anka Virtualization/run/_index.md" >}}

{{< hint info >}}
If the VM is in a _stopped_ state, [`anka run`]({{< relref "arm/Anka Virtualization/command-reference.md#run" >}}) will automatically start it.
{{< /hint >}}

You can use `anka run` _on the host terminal_ to validate things are working properly:

```shell
❯ anka run 12.0-beta bash -c "hostname && ls -l && ping -c 5 google.com"
12-0-beta.local
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


{{< hint info >}}
The [`anka run`]({{< relref "arm/Anka Virtualization/command-reference.md#run" >}}) command doesn't source `.profile`, `.bash_profile`, or `.zshrc` by default. You have to source the file before executing other commands.

```shell
❯ anka run 12.0-beta bash -c "env | grep INSIDE"

❯ anka run 12.0-beta bash -c "source ~/.zshrc; env | grep INSIDE"
INSIDE_ZSHRC=true
```
{{< /hint >}}


{{< hint info >}}
To inherit the host's environment, use the `anka run -E` (existing VM variables will not be overridden by host's variables) or `-e MYENV` options. You can also pass them inside of a file like `anka run --env-file environment.txt`, where environment.txt is a text file in the form `VARIABLE=VALUE`, one variable per line.
{{< /hint >}}


{{< hint info >}}
An advanced usage example of `anka run` inside of a bash script can be found in our [Getting Started Repo's Tag Creation Script.](https://github.com/veertuinc/getting-started/blob/master/create-vm-template-tags.bash)
{{< /hint >}}

## Anka View

{{< hint warning >}}
The anka viewer requires an active UI session on the host.
{{< /hint >}}

{{< hint warning >}}
The `anka view` command currently will only function if you at some point started the VM with `anka start -uv`.
{{< /hint >}}

{{< include file="_partials/arm/Anka Virtualization/view/_index.md" >}}

## SSH

{{< include file="_partials/arm/Anka Virtualization/modify/add/port/_example.md" >}}

## VNC

Once you've enabled Apple's Remote Login inside of the VM, simply add a forwarded port for guest:5900 to the host. You can then VNC from the host into the VM using this port.

## Answers to Frequently Asked Questions

- [`anka run`]({{< relref "arm/Anka Virtualization/command-reference.md#run" >}}) doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool:
  ```shell
  ❯ anka run VNMANE whoami > /dev/null

  ❯ cat file-on-host.txt | anka run 12.0-beta md5
  ff1a596f13d348b63218078c6598ab5e
  ```

## What's next?

- [Modifying your VM Template]({{< relref "arm/Getting Started/modifying-your-vm-template.md" >}})
