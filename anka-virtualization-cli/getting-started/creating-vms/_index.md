---
title: "Creating VMs"
linkTitle: "Creating VMs"
weight: 2
description: >
  Step by step on how to create VMs
aliases:
  - "/apple/getting-started/creating-your-first-vm/"
  - "/intel/getting-started/creating-your-first-vm/"
---

## On this page

- [Quick start (CLI)](#quick-start-cli-recommended)
- [Prerequisites](#prerequisites)
- [Create with anka create](#create-with-anka-create)
- [Related topics](#related-topics)

---

## Quick start (CLI, recommended)

1. [Install Anka]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}}) and [activate your license]({{< relref "anka-virtualization-cli/getting-started/activating-your-anka-license.md" >}}).
2. Confirm your target macOS version is supported: [Supported macOS versions]({{< relref "anka-virtualization-cli/getting-started/creating-vms/supported-macos-versions.md" >}}).
3. Create a VM (downloads IPSW, runs click scripts, installs macOS):

```bash
anka create --list                    # see available guest versions
anka create my-ci-vm 26.6.2           # VM name, then macOS version from --list
```

4. When `anka create` finishes, the VM is **stopped**. User `anka` / password `admin`, SIP off, and VNC are already configured.
5. [Access the VM]({{< relref "anka-virtualization-cli/getting-started/accessing-your-vm.md" >}}), [modify resources]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}}), then [optimize]({{< relref "anka-virtualization-cli/getting-started/creating-vms/optimizing-your-vm.md" >}}) before pushing to a registry.

Creation can take 20–60+ minutes depending on download speed and macOS version. Use `anka --debug create ...` for verbose output.

{{< hint info >}}
**Terms:** A **VM** is the local object on the host. A **tag** is a named version of that VM. A **template** is what you store in the Anka Registry for CI. See [VM clones and tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms/vm-clones-and-tags.md" >}}).
{{< /hint >}}

---

## Prerequisites

{{< hint error >}}
`anka` commands run as **your current user**. Root and non-root users have separate VM libraries. Move VMs with [export/import]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md" >}}), the [Anka Registry]({{< relref "anka-build-cloud/_index.md" >}}), or the Anka app.
{{< /hint >}}

1. [Anka Virtualization is installed.]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
2. **Network:** The host needs outbound access to Apple endpoints for IPSW download and setup. See [Apple's documentation](https://support.apple.com/101555) and whitelist required CDN URLs in corporate firewalls.
   - `http_proxy` / `https_proxy` do **not** work for `anka create`.
   - If you must block network during create, use `ANKA_NETWORK_DISCONNECTED=true` (see [creation troubleshooting]({{< relref "anka-virtualization-cli/troubleshooting/cli/anka-create-stuck-or-failing.md" >}})).
   - Antivirus (CrowdStrike, and similar) often blocks VM startup; disable or whitelist Anka.
3. **Hardware:** The host must support the guest macOS version you want. See [Supported macOS versions]({{< relref "anka-virtualization-cli/getting-started/creating-vms/supported-macos-versions.md" >}}).
4. **ARM only:** Use `.ipsw` files, not `.app` installers. Do not run `sudo anka create` over SSH; use VNC + Terminal or create as your normal user.
5. **ARM only:** Guests built on macOS 15.x hosts do not run on 14.x hosts (14.x → 15.x is fine). This may be true with other macOS versions as it's an incompatibility with host level APIs.

{{< hint info >}}
**Headless hosts (CI nodes, SSH-only):** After every host reboot, unlock the login keychain for the user that runs `anka` before `anka create` or `anka start`. Apple's Virtualization APIs cannot access the keychain while it is locked, and VM create/start may fail (for example, "The virtual machine failed to start" or status 70). SSH alone does not unlock the keychain.

```bash
security unlock-keychain -p "${PW}" login.keychain-db
```

You do not need a GUI desktop session if you run this command (for example from a LaunchDaemon or your CI boot script).

**When you can skip this:** On a Mac where someone already signed in at the desktop after reboot, the login keychain is usually unlocked and this step is not needed.
{{< /hint >}}

---

## Create with `anka create`

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_example.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_extra.md" >}}

After `anka create`, Anka sets up macOS, creates user `anka` with password `admin`, disables SIP, enables VNC, and stops the VM.

---

## Related topics

Child pages in this section:

| Topic | Page |
|-------|------|
| Supported macOS versions | [Supported macOS versions]({{< relref "anka-virtualization-cli/getting-started/creating-vms/supported-macos-versions.md" >}}) |
| Anka.app (GUI) | [Creating VMs (GUI)]({{< relref "anka-virtualization-cli/getting-started/creating-vms/creating-vms-with-anka-gui.md" >}}) |
| Click Scripts feed | [Click Scripts feed]({{< relref "anka-virtualization-cli/getting-started/creating-vms/anka-click-scripts-feed.md" >}}) |
| Clones, tags, templates | [Clones and tags]({{< relref "anka-virtualization-cli/getting-started/creating-vms/vm-clones-and-tags.md" >}}) |
| Export and import | [Export and import]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md" >}}) |
| Host directory mounts | [Host directory mounts]({{< relref "anka-virtualization-cli/getting-started/creating-vms/sharing-host-directories.md" >}}) |
| Post-create tuning | [Optimizing your VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms/optimizing-your-vm.md" >}}) |
| Build Cloud registry | [Registry VM Templates and Tags]({{< relref "anka-build-cloud/getting-started/registry-vm-templates-and-tags.md" >}}) |
| Creation failures | [VM creation is stuck or failing]({{< relref "anka-virtualization-cli/troubleshooting/cli/anka-create-stuck-or-failing.md" >}}) |

---

## What's next?

- [Accessing your VM]({{< relref "anka-virtualization-cli/getting-started/accessing-your-vm.md" >}})
- [Modifying your VM]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md" >}})
