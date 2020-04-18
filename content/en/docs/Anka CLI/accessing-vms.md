---
date: 2019-12-12T00:00:00-00:00
title: "Accessing VMs"
linkTitle: "Accessing VMs"
weight: 4
description: >
  Several methods for accessing your Anka VMs 
---

> [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) requires Remote Login to be enabled for the user.

> If the VM is in a _suspended_ or _stopped_ state, [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) will start it.

There are currently two methods of accessing Anka VMs:

1. Use [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) to pass in commands from the host terminal.
2. Use [`anka view`]({{< relref "docs/Anka CLI/command-reference.md#view" >}}) to launch the Anka Viewer.

## Executing commands through [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}})

{{< include file="shared/content/en/docs/Anka CLI/partials/run/_index.md" >}}

Similar to `docker exec`, [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) allows execution of commands inside of a VM.

When running [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) with no options/flags, it will mount the current folder from the host into the VM and execute commands in this mount point. This can be disabled using the [`-n`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) option.

> Returns 125 on timeout

### Examples

```shell
sudo anka run <template> sudo whoami
```

```shell
HELPERS="set -exo pipefail;"
ANKA_RUN="sudo anka run -N -n"
$ANKA_RUN <template> bash -c "$HELPERS cd /tmp && rm -f /tmp/OpenJDK* && \
  curl -L -O https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u242-b08/OpenJDK8U-jdk_x64_mac_hotspot_8u242b08.pkg && \
  [ \$(du -s /tmp/OpenJDK8U-jdk_x64_mac_hotspot_8u242b08.pkg | awk '{print \$1}') -gt 190000 ] && \
  sudo installer -pkg /tmp/OpenJDK8U-jdk_x64_mac_hotspot_8u242b08.pkg -target / && \
  [[ ! -z \$(java -version 2>&1 | grep 1.8.0_242) ]] && \
  rm -f /tmp/OpenJDK8U-jdk_x64_mac_hotspot_8u242b08.pkg"
```

It's also possible to specify the directory to mount inside of the VM using this syntax `-v /host/folder:/mnt/path`: 

```shell
sudo anka run -v $PWD:/tmp/mountpoint_1 <template> pwd
```

> Currently, only a single directory can be specified. If you need more than one folder shared between host and the VM, use the [`anka mount`]({{< relref "docs/Anka CLI/command-reference.md#mount" >}}) command:

```shell
sudo anka mount <template> ~/Library/MobileDevice/Provisioning\ Profiles/ /Users/anka/Library/MobileDevice/Provisioning\ Profiles/
sudo anka run <template> xcodebuild -exportOptionsPlist exportInfo.plist archive
```

### Working with environment variables

You can pass them inside of a file like `anka run -f environment.txt`, where environment.txt is a text file in the form `VARIABLE=VALUE`, one variable per line.

To inherit the host's environment, use `anka run -E` command. However, existing VM variables will not be overridden by host's variables.

The [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) command doesn't source .profile or .bash_profile. You have to source the file before executing other commands:

```shell
sudo anka run ios-temp bash -c 'source ~/.bash_profile; ./iPhone/make_build -b dev -c 1 -d ./build -e Mobile -z'`
```
### Other examples

`anka run sierrav40c1 xcodebuild -sdk iphonesimulator -scheme Kickstarter-iOS build`  - Will mount the default directory from the host into the sierrav40c1 Vm and execute build.

`anka run -w /Applications <template> ls -l`  - Will pipe the results of `ls -l` from the VM's `/Applications` directory.

`anka run -v . <template> ls`  - Will mount the host current directory inside the VM, execute `ls`, pipe the results and unmount.

`anka run -v . <template> xcodebuild ...`  - Will mount the current directory from the developer machine(host) to the VM and execute an `xcodebuild` command and pipe the results back.

`anka run sudo ...`  - Executes commands inside the VM with `sudo` privileges. For instance:

`anka run <template> cp -R simpledir /Users/anka`  - Will copy the current host directory to the VM at /Users/anka location

`anka run -n sierrav40c1 xcodebuild -sdk iphonesimulator -scheme Kickstarter-iOS build`  - Will not mount the current host directory and execute build in the VM current directory

You can write parameters to VM configuration so they're available on execution with [`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) using the [`anka modify`]({{< relref "docs/Anka CLI/command-reference.md#modify" >}}) command.

[`anka run`]({{< relref "docs/Anka CLI/command-reference.md#run" >}}) doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool:
```shell
anka run -n VNMANE whoami > /dev/null

cat file.txt | anka run -n <template> md5
```

## Launching the Anka Viewer with [`anka view`]({{< relref "docs/Anka CLI/command-reference.md#view" >}})

{{< include file="shared/content/en/docs/Anka CLI/partials/view/_index.md" >}}

```shell
❯ sudo anka start 10.15.4
+-----------------------+-------------------------------------------------------------------+
| uuid                  | c0847bc9-5d2d-4dbc-ba6a-240f7ff08032                              |
+-----------------------+-------------------------------------------------------------------+
| name                  | 10.15.4 (base:port-forward-22:brew-git:jenkins:openjdk-1.8.0_242) |
+-----------------------+-------------------------------------------------------------------+
| created               | Apr 16 16:17                                                      |
+-----------------------+-------------------------------------------------------------------+
| cpu_cores             | 2                                                                 |
+-----------------------+-------------------------------------------------------------------+
| ram                   | 4G                                                                |
+-----------------------+-------------------------------------------------------------------+
| display               | 1                                                                 |
+-----------------------+-------------------------------------------------------------------+
| hard_drive            | 80Gi (16.6Gi on disk)                                             |
+-----------------------+-------------------------------------------------------------------+
| addons_version        | 2.2.2.116                                                         |
+-----------------------+-------------------------------------------------------------------+
| status                | running                                                           |
+-----------------------+-------------------------------------------------------------------+
| vnc_connection_string | vnc://:admin@192.168.0.107:5900                                   |
+-----------------------+-------------------------------------------------------------------+

> sudo anka view 10.15.4 
```
You should now see the Anka Viewer window:

![Anka Viewer](/images/anka-view-viewer.png)

> You can set the resolution of the Viewer using `sudo anka modify 10.15.4 set display -r 1200x800`