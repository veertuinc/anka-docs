---
date: 2019-12-12T00:00:00-00:00
title: "Starting and Accessing your VM"
linkTitle: "Starting and Accessing your VM"
weight: 3
description: >
  Various methods for starting and accessing your Anka VM
---

## Prerequisites

1. [You've installed the Anka Virtualization package]({{< relref "docs/Getting Started/installing-the-anka-virtualization-package.md" >}})
2. [You've got an active license]({{< relref "docs/Licensing/_index.md" >}})
3. [You've created your first VM Template]({{< relref "docs/Getting Started/creating-your-first-vm.md" >}})

> When running Anka commands on your machine, ensure that you have an active login session (you might have to VNC in if you're connected over SSH) and also have enabled automatic login for the current user: `System Preferences > Users > Enable automatic login`

## Anka Start

### With the UI

Launch the Anka application found under /Applications. Once launched, you will see the VM Template on the right sidebar. You can double click the VM to not only start it but also launch the Anka Viewer window.

![ui with vm in the sidebar list](/images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

### With the CLI

To start the VM in headless mode: `sudo anka start {vmNameOrUUID}`

> `sudo anka start -v {vmNameOrUUID}` will start the VM and launch the Anka Viewer window.

```shell
❯ anka start 11.0.0-beta5
+-----------------------+--------------------------------------+
| uuid                  | 55b27ccc-81e2-47e3-9702-e678540f7219 |
+-----------------------+--------------------------------------+
| name                  | 11.0.0-beta5                         |
+-----------------------+--------------------------------------+
| created               | Sep 01 08:30                         |
+-----------------------+--------------------------------------+
| cpu_cores             | 4                                    |
+-----------------------+--------------------------------------+
| ram                   | 8G                                   |
+-----------------------+--------------------------------------+
| display               | 1                                    |
+-----------------------+--------------------------------------+
| hard_drive            | 128Gi (17.9Gi on disk)               |
+-----------------------+--------------------------------------+
| addons_version        | 2.3.0                                |
+-----------------------+--------------------------------------+
| status                | running                              |
+-----------------------+--------------------------------------+
| mac                   | 76:f5:d8:dd:63:bc                    |
+-----------------------+--------------------------------------+
| vnc_connection_string | vnc://192.168.0.107:5900             |
+-----------------------+--------------------------------------+
```

## Anka Run (exec)

Similar to `docker exec`, [`anka run`]({{< relref "docs/Anka Virtualization/command-reference.md#run" >}}) allows execution of commands inside of a VM.

{{< include file="shared/content/en/docs/Anka Virtualization/partials/run/_index.md" >}}

> If the VM is in a _suspended_ or _stopped_ state, [`anka run`]({{< relref "docs/Anka Virtualization/command-reference.md#run" >}}) will start it.

> When running [`anka run`]({{< relref "docs/Anka Virtualization/command-reference.md#run" >}}) with no options/flags, it will mount the current folder from the host into the VM and execute commands in this mounted location. This can be disabled using the [`-n`]({{< relref "docs/Anka Virtualization/command-reference.md#run" >}}) option.

Once started, you can use `anka run` _on the host terminal_ to validate things are working properly:

```shell
❯ anka run 11.0.0-beta5 bash -c "hostname && ls -l && ping -c 5 google.com"
Mac-mini.local
total 102872
drwxr-xr-x   13 anka  staff       416 Aug 31 10:35 _diag
drwxr-xr-x    7 anka  staff       224 Aug 31 10:35 _work
-rw-r--r--    1 anka  staff  52630785 Aug 28 10:12 actions-runner-osx-x64-2.273.0.tar.gz
drwxr-xr-x  230 anka  staff      7360 Aug 19 07:49 bin
-rwxr-xr-x    1 anka  staff      2673 Aug 19 07:48 config.sh
-rwxr-xr-x    1 anka  staff       623 Aug 19 07:48 env.sh
drwxr-xr-x    3 anka  staff        96 Aug 19 07:49 externals
-rwxr-xr-x    1 anka  staff      1666 Aug 19 07:48 run.sh
-rwxr-xr-x    1 anka  staff      3280 Aug 28 10:12 svc.sh
PING google.com (172.217.10.110): 56 data bytes
64 bytes from 172.217.10.110: icmp_seq=0 ttl=114 time=18.919 ms
64 bytes from 172.217.10.110: icmp_seq=1 ttl=114 time=24.834 ms
64 bytes from 172.217.10.110: icmp_seq=2 ttl=114 time=16.992 ms
64 bytes from 172.217.10.110: icmp_seq=3 ttl=114 time=23.158 ms
64 bytes from 172.217.10.110: icmp_seq=4 ttl=114 time=25.797 ms

--- google.com ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 16.992/21.940/25.797/3.416 ms
```

> The [`anka run`]({{< relref "docs/Anka Virtualization/command-reference.md#run" >}}) command doesn't source .profile or .bash_profile. You have to source the file before executing other commands

> To inherit the host's environment, use `anka run -E` command. However, existing VM variables will not be overridden by host's variables. You can also pass them inside of a file like `anka run --env-file environment.txt`, where environment.txt is a text file in the form `VARIABLE=VALUE`, one variable per line.

> An advanced usage example of `anka run` inside of a bash script can be found [HERE](https://github.com/veertuinc/getting-started/blob/master/ANKA_BUILD_CLOUD/create-tags.bash)

## Anka View

{{< include file="shared/content/en/docs/Anka Virtualization/partials/view/_index.md" >}}

{{< imgwithlink src="/images/getting-started/starting-and-accessing-your-vm/anka-start-view-viewer-window.png" >}}

> You can set the resolution of the VM under the `Apple Menu > View`. Or, you can use the Live Resolution and Enter Fullscreen.

## SSH

In order to SSH into the VM, you'll need to enable **Remote Login** under **System Preferences > Sharing** (enabled by default). Next, check that networking has fully started on the VM by running `anka show {vmNameOrUUID}`:

```shell
❯ anka show 11.0.0-beta5
+-----------------------+--------------------------------------+
| uuid                  | 55b27ccc-81e2-47e3-9702-e678540f7219 |
+-----------------------+--------------------------------------+
| name                  | 11.0.0-beta5                         |
+-----------------------+--------------------------------------+
| created               | Sep 01 08:30                         |
+-----------------------+--------------------------------------+
| cpu_cores             | 4                                    |
+-----------------------+--------------------------------------+
| ram                   | 8G                                   |
+-----------------------+--------------------------------------+
| display               | 1                                    |
+-----------------------+--------------------------------------+
| hard_drive            | 128Gi (18.2Gi on disk)               |
+-----------------------+--------------------------------------+
| addons_version        | 99.9.0.118.19306                     |
+-----------------------+--------------------------------------+
| status                | running                              |
+-----------------------+--------------------------------------+
| ip                    | 192.168.64.49                        |
+-----------------------+--------------------------------------+
| mac                   | 76:f5:d8:dd:63:bc                    |
+-----------------------+--------------------------------------+
| vnc_connection_string | vnc://192.168.0.107:5900             |
+-----------------------+--------------------------------------+
```

If you do not see the row **ip**, then networking has not fully been started yet.

Once you do see an ip, you can then SSH with the user and ip: `ssh anka@{ip}`

{{< imgwithlink src="/images/getting-started/starting-and-accessing-your-vm/anka-show-remote-login-and-ssh.png" >}}

> We provide a fixed IP inside of the VM for accessing the host: `192.168.64.1` (or `192.168.128.1` for "host" type).

## Answers to Frequently Asked Questions

- [`anka run`]({{< relref "docs/Anka Virtualization/command-reference.md#run" >}}) doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool:
    ```shell
    anka run -n VNMANE whoami > /dev/null
    cat file.txt | anka run -n {vmNameOrUUID} md5
    ```
- You can set the resolution of the Anka Viewer using `sudo anka modify 10.15.4 set display -r 1200x800`
- [Port forwarding of VM ports is supported]({{< relref "docs/Anka Virtualization/command-reference.md#example---add-port-forwarding" >}})