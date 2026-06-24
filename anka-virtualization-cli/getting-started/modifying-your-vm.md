---
title: "Modifying your VM"
linkTitle: "Modifying your VM"
weight: 4
description: >
  Modify your VM using the Anka Virtualization CLI
---

## Prerequisites

1. [You've installed the Anka Virtualization package.]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
2. [You've created your first VM.]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})

---

{{< hint warning >}}
The rest of this Getting Started guide focuses heavily on the (Command-Line Interface). These will be performed from within your [macOS Terminal](https://support.apple.com/guide/terminal/welcome/mac). For all available CLI commands, flags, and options, see the [Command Reference]({{< relref "anka-virtualization-cli/command-line-reference.md" >}}).
{{< /hint >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/_index.md" >}}

## Recommended VM Resources

The goal is to give you a starting point for CPU and RAM on your [VM Template]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#vm-templates" >}}), based on how many VMs you plan to run at once. Apple does not permit more than 2 VMs per machine (this is only a strict limitation on ARM machines currently).

### Intel

To calculate the cores available for VMs, you'll take the number of physical cores and then multiply it by 2. Once you have the total virtual cores, you can assign all of them (maybe - 1 or 2 CPUs to give the host breathing room) if you plan to run a single VM on the host. Otherwise, if you want to run two VMs at once, you'll divide the total virtual cores by 2 and then assign this number to the [VM Template]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#vm-templates" >}}). RAM follows a similar pattern, giving 2GB for the host: `totalRAMGB - 2GB` and `(totalRAMGB / 2) - 2GB`.

### ARM

Use the host's total CPU core count from [Apple's tech specs](https://www.apple.com/mac-mini/specs/) or `sysctl hw.physicalcpu`. Apple schedules guest vCPUs across the available cores — you do not need to account for performance vs efficiency cores separately. Note, though, that Apple mixes performance and efficiency cores, so the VMs are not guaranteed to be allocated to performance cores only.

#### CPU

1. **Find the host's total core count** (`C`):
   - On the host: `sysctl hw.physicalcpu`
   - Or use the CPU core count listed in Apple's specs for your chip (e.g., an M4 Mac mini listed as 10-core → `C = 10`).

2. **Assign vCPUs to each VM template:**
   - **1 VM:** `C`, or subtract 1–2 if you want headroom for the host
   - **2 VMs:** `C / 2`
   - **Minimum:** 4 vCPUs per VM. Values below 4 can cause instability inside the VM.

{{< hint warn >}}
You can overcommit slightly for a 2 VM setup. But we cannot guarantee stability in these situations.
{{< /hint >}}

#### RAM

RAM follows the same pattern as Intel:

- **1 VM:** `totalRAMGB - 2GB`
- **2 VMs:** `(totalRAMGB / 2) - 2GB`
- **Mac Studio hosts:** cap per-VM RAM at 60GB (Apple virtualization limit)

#### Worked examples

The following hosts show the formulas applied:

{{< rawhtml >}}
<div style="display:flex;">
<table>
<tbody>
  <tr style="text-align:center">
    <td style="font-size: 1.3rem; background-color: #f2e6ff;"><b>Example host</b></td>
    <td style="background-color: #f2e6ff;"><b>Cores</b></td>
    <td style="background-color: #f2e6ff;"><b>1 VM (CPUs)</b></td>
    <td style="background-color: #f2e6ff;"><b>2 VMs (CPUs each)</b></td>
    <td style="background-color: #f2e6ff;"><b>RAM (32GB host)</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>M4 Mac mini</b><br />10-core</td>
    <td>10</td>
    <td>8–10</td>
    <td>5-6</td>
    <td>30GB / 14GB</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>M4 Pro Mac mini</b><br />14-core</td>
    <td>14</td>
    <td>12–14</td>
    <td>7-8</td>
    <td>30GB / 14GB</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>M4 Max Mac Studio</b><br />16-core</td>
    <td>16</td>
    <td>14–16</td>
    <td>8-9</td>
    <td>30GB / 14GB (max 60GB)</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>M2 Ultra Mac Studio</b><br />24-core</td>
    <td>24</td>
    <td>22–24</td>
    <td>12-13</td>
    <td>30GB / 14GB (max 60GB)</td>
  </tr>
</tbody>
</table>
</div>
{{< /rawhtml >}}

## Common Examples

### VM Resources

#### CPU

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/cpu/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/cpu/_example.md" >}}

#### RAM

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/ram/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/ram/_example.md" >}}

#### DISK

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/disk/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/disk/_extra.md" >}}


### Port forwarding from guest to host

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/port/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/port/_example.md" >}}

### Changing your VM's network configuration

Depending on your network topology, there are instances where you might need to use a bridge mode and assign your VM a unique IP address instead of the default shared IP of the host:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/network/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/network/_extra.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/network/_example.md" >}}

### Modify the hardware UUID and Serial with `custom-variable` (intel only)

At times you might need to modify the hardware serial or UUID to run proper builds/tests that require specific hardware to function properly:

```shell
> sudo anka modify 12.3.1 set custom-variable --help
Usage: anka modify set custom-variable [OPTIONS] KEY VALUE

  Set custom nvram & smb variables (Example: . . . set custom-variable hw.serial "<SERIAL>"

Options:
  --help  Display usage information
```

```shell
sudo anka modify {vmNameOrUUID} set custom-variable hw.uuid "GUID"
sudo anka modify {vmNameOrUUID} set custom-variable hw.serial 'MySerial'
```

---

## What's next?

- [Understanding VM Networking]({{< relref "anka-virtualization-cli/getting-started/understanding-vm-networking.md" >}})
