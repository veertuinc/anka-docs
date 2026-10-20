---
date: 2019-12-12T00:00:00-00:00
title: "Modifying the Global Configuration"
linkTitle: "Modifying the Global Configuration"
weight: 4
description: >
  How to modify the Global Anka Virtualization configuration
---

{{< include file="_partials/anka-virtualization-cli/command-line-reference/config/_index.md" >}}

## Viewing the configuration

View all default configuration settings for Anka installation on the host with `anka config` command:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/config/_example.md" >}}

> You can see a value for a specific configuration parameter with `anka config $PARAM_NAME`.

- The default VM username is `anka`
- The default VM password is `admin`
- The default storage locations for Anka VM Templates & Tags are:
  - vm_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/vm_lib/`
  - state_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/state_lib/`
  - img_lib_dir - `/Users/XXX/Library/Application Support/Veertu/Anka/img_lib/`
- The log directory is `/Users/XXX/Library/Logs/Anka/` and log file is `anka.log`.

## Registry push and pull settings

| Setting | Purpose |
|---------|---------|
| `push_mode` | How `anka push` stores a tag in an OCI registry. `1` (default) stores the full tag. `2` stores only the file changes since the base tag. Prefer `--thin` for tool tags so you do not change this setting. The Anka Registry ignores this setting. See [Thin pushes and pulls]({{< relref "templates-and-registries/templates-and-tags.md#thin-pushes-and-pulls" >}}). |
| `ankaignore` | The file that lists the paths that a push or export with `--thin` or `push_mode` `2` does not include. See [Exclude files from a push]({{< relref "templates-and-registries/templates-and-tags.md#exclude-files-from-a-push" >}}). |
| `default_vmid` | The VM that `anka pull <url>` clones when you do not give a VM name. See [Pull without a VM name]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md#pull-without-a-vm-name" >}}). |

These environment variables change the same behavior for one command:

- `ANKA_PUSH_MODE` sets `push_mode` for one command.
- `ANKA_ANKAIGNORE` sets `ankaignore` for one command.
- `ANKA_COMPRESSION_LEVEL` above `0` turns on gzip for the OCI Container Registry Protocol and sets the zlib level. `1` is the fastest level.

## VM swap directory (ARM)

On ARM hosts, set `swap_dir` to a fast local path when you want guest swap on host disk instead of inside the VM image. This is common on [AWS EC2 Mac with local storage]({{< relref "aws-ec2-mac/using-local-storage.md#step-5-optional--host-swap-directory-for-vm-memory-pressure" >}}).

```bash
anka config swap_dir /Volumes/Anka/swap
```

Per-VM overrides use the `ANKA_SWAP_DIR` environment variable on `anka start`. Guest addons 3.3.9+ are required.

## Changing the default Anka VM storage location

Depending on how many Anka VMs you have, the disk usage might be too much for the default storage location. There are three configuration parameters to control location for storing Anka VMs.

{{< hint warning >}}
Anka 3.x and greater requires a APFS volume.
{{< /hint >}}

{{< hint info >}}
It's recommended to keep the vm_lib_dir on the local disk as it contains file locks.
{{< /hint >}}

Assuming you want to store your Templates on **/Volumes/ExternalDrive/**, perform these steps:

1. `anka config img_lib_dir /Volumes/ExternalDrive/img_lib`
2. `anka config state_lib_dir /Volumes/ExternalDrive/state_lib`
3. `anka config vm_lib_dir /Volumes/ExternalDrive/vm_lib`
4. Go under System Preferences > Security & Privacy > Full Disk Access and add `/Applications/Anka.app/Contents/MacOS/ankahv-arm64.app/Contents/MacOS/ankahv-arm64` (or `ankahv-x86_64.app` on intel) as well as `/Library/Application\ Support/Veertu/Anka/bin/anka_agent`. Make sure the slider button is enabled too!
