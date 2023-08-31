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

Over time, this information may become out of date. The goal is to provide you with an idea of the CPU and RAM to set for VMs depending on the amount you wish to run. Note that Apple does not permit more than 2 VMs per machine (this is only a strict limitation on ARM machines currently).

### Intel

To calculate the cores available for VMs, you'll take the number of physical cores and then multiply it by 2. Once you have the total virtual cores, you can assign all of them (maybe - 1 or 2 CPUs to give the host breathing room) if you plan to run a single VM on the host. Otherwise, if you want to run two VMs at once, you'll divide the total virtual cores by 2 and then assign this number to the [VM Template]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#vm-templates" >}}). RAM follows a similar pattern, giving 2GB for the host: `totalRAMGB - 2GB` and `(totalRAMGB / 2) - 2GB`.

### ARM

Things are similar with slightly different CPUs calculations due to the different CPU types:

{{< rawhtml >}}
<div style="display:flex;">
<table>
<tbody>
  <tr style="text-align:center">
    <td style="font-size: 1.3rem; background-color: #f2e6ff;"><b>Apple/ARM</b></td>
    <td style="background-color: #f2e6ff;"><b>1VM</b></td>
    <td style="background-color: #f2e6ff;"><b>2VMs</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Mini, M1/M2</b><br />8-core</td>
    <td><b>CPUs:</b> 6 or 8-cores<br /><b>RAM:</b> totalRAMGB - 2GB</td>
    <td><b>CPUs:</b> 4-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Mini, M2 Pro</b><br />10-core</td>
    <td><b>CPUs:</b> 8 or 10-cores<br /><b>RAM:</b> totalRAMGB - 2GB</td>
    <td><b>CPUs:</b> 6-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Mini, M2 Pro</b><br />12-core</td>
    <td><b>CPUs:</b> 10 or 12-cores<br /><b>RAM:</b> totalRAMGB - 2GB</td>
    <td><b>CPUs:</b> 8-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Studio, M2 Max</b><br />12-core</td>
    <td><b>CPUs:</b> 10 or 12-cores<br /><b>RAM:</b> totalRAMGB - 2GB (max of 60GB)</td>
    <td><b>CPUs:</b> 8-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB (max of 60GB)</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Studio, M2 Ultra</b><br />24-core</td>
    <td><b>CPUs:</b> 10 or 12-cores<br /><b>RAM:</b> totalRAMGB - 2GB (max of 60GB)</td>
    <td><b>CPUs:</b> 10 or 12-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB (max of 60GB)</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Studio, M1 Max, 2022</b><br />10-core</td>
    <td><b>CPUs:</b> 8-cores<br /><b>RAM:</b> totalRAMGB - 2GB (max of 60GB)</td>
    <td><b>CPUs:</b> 6 or 8-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB (max of 60GB)</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>Mac Studio, M1 Ultra, 2022</b><br />20-core</td>
    <td><b>CPUs:</b> 10-cores<br /><b>RAM:</b> totalRAMGB - 2GB (max of 60GB)</td>
    <td><b>CPUs:</b> 6 or 8-cores<br /><b>RAM:</b> (totalRAMGB / 2) - 2GB (max of 60GB)</td>
  </tr>
</tbody>
</table>
</div>
{{< /rawhtml >}}

{{< hint info >}}
Due to Ultra using the NUMA architecture, VMs/virtualization will only ever use 8 performance cores at a time.
{{< /hint >}}

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
