---
date: 2018-03-06
title: "Release Notes"
weight: 100
---

{{< hint warning >}}
Not all plugins are maintained by Veertu Inc developers. You might not see them listed here.
{{< /hint >}}

## Current Version

### 3.1.0 (3.1.0.151) - Oct 17th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** MacOS installation through the `anka create` command is now automated.
    - SIP is now disabled by default inside of the VM.
    - VNC is now enabled by default inside of the VM.
- **Improvement:** VMs can be started in Recovery Mode from the CLI with `ANKA_START_MODE=2 anka start 12.6`.
- **Improvement:** The `anka create` command will now produce `stopped` state VMs (suspending is not supported by Apple yet).
- **Improvement:** [Shortened `anka modify` and `port-forwarding` command.]({{< relref "Whats New/anka-3.1.0/index.md#shortened-anka-modify-and-port-forwarding-command" >}})
- **Improvement:** Better handling of VM kernel panics.
- **Improvement:** Improved network performance and security.
- **Improvement:** The `anka list` command will now show the last accessed date of the VM.
- **Improvement:** Pulling an intel VM Template from the Registry will now throw a failure.
- **Improvement:** Anka license commands will warn about EULA needing to be accepted.
- **New Feature:** [The `anka create --list` command will display macOS versions available to target with `anka create -a` (which downloads the appropriate .ipsw and uses it for creation).]({{< relref "Whats New/anka-3.1.0/index.md#targeting-of-specific-macos-versions-with-anka-create--a" >}})
- **New Feature:** [You can now resize the VM's disk.]({{< relref "Whats New/anka-3.1.0/index.md#ability-to-resize-the-vms-disk" >}})
- **New Feature:** [Support for the Anka Build Cloud's UAKs when interacting with the registry commands.]({{< relref "Whats New/anka-3.1.0/index.md#support-for-the-anka-build-clouds-uaks-when-interacting-with-the-registry-commands" >}})
- **New Feature:** `ANKA_LOG_LEVEL="debug"` is available as a replacement for `anka --debug`.
- **Bug Fix:** `anka push -r` was not working to specify registry url to push to.
- **Bug Fix:** Disabled beta program enrollment for macOS.

## Previous Versions

### 3.0.1 (3.0.1.144) - May 12th 2022

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
