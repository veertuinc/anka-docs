---
title: "Creating VMs with Anka.app"
linkTitle: "Creating VMs (GUI)"
weight: 2
description: >
  Create VMs using the Anka desktop application instead of anka create
aliases:
  - "/anka-virtualization-cli/getting-started/creating-vms-with-anka-gui/"
---

{{< hint warning >}}
**CLI creation is recommended.** `anka create` installs macOS, creates user `anka` / password `admin`, disables SIP, and enables VNC automatically. The GUI path requires manual setup. See [Creating VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms/_index.md" >}}).
{{< /hint >}}

## Steps

1. Click **Create new VM**.
2. **Leave the installer blank** and click **Options** for non-default values.

{{< hint info >}}
A blank installer targets the latest macOS from Apple's CDN (`updates.cdn-apple.com`). Use your own `.ipsw` with the CLI if you need a specific version.
{{< /hint >}}

![installer with pkg]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/ui.png)

3. Wait for creation to finish. The VM appears in the sidebar.

![ui with vm in the sidebar list]({{< siteurl >}}images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

## Post-creation setup (required)

The GUI does **not** run the same automation as `anka create`.

1. Start the VM with `anka start -uv` to open the [Anka Viewer]({{< relref "anka-virtualization-cli/command-line-reference.md#view" >}}) and mount addons. **Do not use `-uv` in scripts**; it is for interactive sessions only.

   - `anka view` only works after start if you used `-v`.
   - **ARM:** `sudo anka view` as a non-root user is not supported yet. Enable VNC for VMs started with `sudo`.

2. In the viewer, finish macOS installation and **install the addons package** from the disk mounted with `-u`.

3. Reboot the VM.

![mounted addons]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/addonspkg.png)

{{< hint warning >}}
For addons and autologin to work, create user **`anka`** with password **`admin`**, or configure autologin manually for your user.
{{< /hint >}}

## Disable SIP (recommended for CI)

SIP is **enabled** for GUI-created VMs only (`anka create` disables it automatically).

With SIP on, commands can hang on permission dialogs, and `syspolicyd` scans processes and uses extra CPU and RAM.

Start recovery mode:

```bash
ANKA_START_MODE=2 anka start my-ci-vm
```

![recovery-mode]({{< siteurl >}}images/apple/getting-started/accessing-your-vm/anka-app-recovery-mode.png)

In Recovery, open **Terminal** and run `csrutil disable`. Stop the VM. SIP is off on the next boot.

## What's next

- [Accessing your VM]({{< relref "anka-virtualization-cli/getting-started/accessing-your-vm.md" >}})
- [Modifying your VM]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}})
