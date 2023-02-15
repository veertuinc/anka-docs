---
date: 2018-03-06
title: "Release Notes"
weight: 100
---

{{< hint warning >}}
Not all plugins are maintained by Veertu Inc developers. You might not see them listed here.
{{< /hint >}}

## Current Version

### 3.2.1 (3.2.1.155) (Intel) - Feb 15th 2023

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

- **Bug Fix:** Marketplace AMI licensing fixes.

### 3.2.1 (3.2.1.157) (ARM) - Feb 15th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Marketplace AMI licensing fixes.

## Previous Versions

### 3.2.0 (3.2.0.153) (Intel) - Jan 9th 2023

{{< hint info >}}
Addons upgrading from Anka 2.x is not required but is recommended.
{{< /hint >}}

- VMs suspended on Anka 2.x will need to be re-suspended on 3.x.
- The Controller Agent version that comes with Anka will start at `1.30.1` (ARM will be `1.22.0`). **Older versions of the Controller do not support Anka 3. You will need to upgrade your controller to at least 1.30.1.**
- Previously, `anka create` would create a suspended VM. Starting in this version, VMs are stopped.
- GUI (Anka.app) VM creation will produce a VM without macOS set up.
- `anka --machine-readable registry list` has a key name change from `id` to `uuid`.
- The `modify set` commands are deprecated and will not show in the `anka modify --help` output. Please migrate to using the new modify commands instead.
- Only macOS 10.15 guest version and above are supported by Veertu. If you're using an older version, do not upgrade addons to 3.x and they will continue to function.

### 3.2.0 (3.2.0.153) (ARM) - Nov 16th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** Network stability (jenkins agents would sometimes suddenly disconnect).
- **Improvement:** Stability for Anka Create macOS setup process (VNC timeout, etc).
- **Improvement:** [Anka Click](https://github.com/veertuinc/anka-click-scripts) script support: Enabling "Reduce transparency" post-`anka create` to avoid dynamical wallpapers and other background changes from breaking image clicking.
- **Improvement:** Partially downloaded files (dot files under `img_lib_dir`) from `anka registry pull` are cleaned up after they are older than 1 day.
- **Bug Fix:** Pull/Conversion is happened on every VM Start request.
- **Bug Fix:** Controller Agent coming with Anka package was being installed and forcefully upgrading the version users already have.
- **Bug Fix:** Anka run was hanging when the VM received a `sudo reboot` (supports packer's expect disconnect feature).
- **Bug Fix:** `ANKA_DEFAULT_USER` and `ANKA_DEFAULT_PASSWD` did not allow users to change the default user and pass in the VM when using `anka create`.

### 3.1.1 (3.1.1.152) (ARM) - Oct 25th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accesing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Pulling was converting each and delaying VM start requests.
- **Bug Fix:** Various anka create automation script fixes.

### 3.1.0 (3.1.0.151) (ARM) - Oct 17th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accesing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** MacOS installation through the `anka create` command is now automated.
    - SIP is now disabled by default inside of the VM.
    - VNC is now enabled by default inside of the VM.
- **Improvement:** VMs can be started in Recovery Mode from the CLI with `ANKA_START_MODE=2 anka start 12.6`.
- **Improvement:** The `anka create` command will now produce `stopped` state VMs (suspending is not supported by Apple yet).
- **Improvement:** [Shortened `anka modify` and `port-forwarding` command.]({{< relref "whats-new/anka-3.1.0/index.md#shortened-anka-modify-and-port-forwarding-command" >}})
- **Improvement:** Better handling of VM kernel panics.
- **Improvement:** Improved network performance and security.
- **Improvement:** The `anka list` command will now show the last accessed date of the VM.
- **Improvement:** Pulling an intel VM Template from the Registry will now throw a failure.
- **Improvement:** Anka license commands will warn about EULA needing to be accepted.
- **New Feature:** [The `anka create --list` command will display macOS versions available to target with `anka create -a` (which downloads the appropriate .ipsw and uses it for creation).]({{< relref "whats-new/anka-3.1.0/index.md#targeting-of-specific-macos-versions-with-anka-create--a" >}})
- **New Feature:** [You can now resize the VM's disk.]({{< relref "whats-new/anka-3.1.0/index.md#ability-to-resize-the-vms-disk" >}})
- **New Feature:** [Support for the Anka Build Cloud's UAKs when interacting with the registry commands.]({{< relref "whats-new/anka-3.1.0/index.md#support-for-the-anka-build-clouds-uaks-when-interacting-with-the-registry-commands" >}})
- **New Feature:** `ANKA_LOG_LEVEL="debug"` is available as a replacement for `anka --debug`.
- **Bug Fix:** `anka push -r` was not working to specify registry url to push to.
- **Bug Fix:** Disabled beta program enrollment for macOS.


### 3.0.1 (3.0.1.144) (ARM) - May 12th 2022

{{< hint info >}}
Addons upgrading is not required for features in this release. However, it's always recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` does not function post-vm-start unless you started the VM with -v.
- In Anka 2 we enabled a minimal VNC by default, however it is not available in Anka 3.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Curling a forwarded port which does not have the inner VM service/port running will no longer crash the VM.
- **Bug Fix:** `anka --machine-readable show` now shows proper #G value under `ram` key's value, instead of bytes.
- **Bug Fix:** EULA acceptance from GUI was not sticking.
- **Bug Fix:** `anka --machine-readable describe` port forwarding rules had a key name of `name` instead of `rule_name` which was in Anka 2.
- **Bug Fix:** `anka --machine-readable describe` port forwarding rules had no `host_port` set which packer was looking for.
- **Bug Fix:** Screenshot features are now available in the CLI and GUI app.
- **Bug Fix:** Cloning two VMs after setting `anka modify` to `bridge` was showing a new `creation_date` and the same IP in use in anka show (but not in the VMs).
- **Bug Fix:** You can now use certs to authenticate with pull and push commands.
- **Bug Fix:** Anka GUI App no longer default to using 2vCPUs if more are available.
- **Bug Fix:** Deleting a tag will now delete the associated .ank files properly.
- **Improvement:** Trying to use .app installers will now throw a clear error that they are not supported.
- **Improvement:** Support for Mac Studio hardware.
- **Improvement:** Custom `vm_lib_dir` will not be automatically created so that it doesn't prevent network volumes from mounting to that location with the proper name on reboot of the host.
- **Improvement:** We now show the internal screen sharing connection string if there is a corresponding port forwarding rule.
- **Improvement:** Expired licenses or licenses that do not support the command you're running will now throw a clear error.
- **New Feature:** Bridge network isolation is now available.

### 3.0.0 (3.0.0.140) - Feb 1st 2022

- This is our first release! Features are detailed in the documentation throughout this site!
