---
date: 2018-03-06
title: "Release Notes"
weight: 100
---

{{< hint warning >}}
Not all plugins are maintained by Veertu Inc developers. You might not see them listed here.
{{< /hint >}}

## Current Version

### 3.0.1 (3.0.1) - X 2022

{{< hint info >}}
Addons upgrading is not required for features in this release. However, it's always recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` does not function post-vm-start unless you started the VM with -v.
- In Anka 2 we enabled a minimal VNC by default, however it is not available in Anka 3.
- iCloud logins will fail inside of the VM.
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
- **Improvement:** Support for Apple Studio hardware.
- **Improvement:** Custom `vm_lib_dir` will not be automatically created so that it doesn't prevent network volumes from mounting to that location with the proper name on reboot of the host.
- **Improvement:** We now show the internal screen sharing connection string if there is a corresponding port forwarding rule.
- **Improvement:** Expired licenses or licenses that do not support the command you're running will now throw a clear error.
- **New Feature:** Bridge network isolation is now available.

## Previous Versions

### 3.0.0 (3.0.0.140) - Feb 1st 2020

- This is our first release! Features are detailed in the documentation throughout this site!
