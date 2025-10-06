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

## Prerequisites / Orientation

{{< hint error >}}
**IMPORTANT:** `anka` CLI VM creation, modifications, etc, are performed under your current user. The root user and non-root users will have different environments. You can use the [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}) or [Anka App]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}}) or `anka export/import` to move VMs between users (and hosts).
{{< /hint >}}

{{< hint warning >}}
**[ARM/Silicon]** Apple has been making lots of changes to the Virtualization APIs in 15.x and beyond. There are requirements for the latest Xcode and sometimes Device Support packages to be installed and configued on the host or else creation will fail. We attempt to indicate what combinations are required to get creation working, but you might need to do some expirimentation to get it working.
{{< /hint >}}

{{< hint warning >}}
Apple's .app installer files are currently not supported on ARM. Instead, you'll need to obtain .ipsw files.
{{< /hint >}}

{{< hint warning >}}
**[ARM/Silicon]** `sudo anka create` over SSH is blocked by Apple's security features. Use VNC > Terminal to create the VM with sudo, or, just create it as the current non-root user.
{{< /hint >}}

1. [You've installed the Anka Virtualization package.]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
2. The host you wish to use has a full internet connection. 
  - If you are behind a corporate firewall/proxy, you'll need to review https://support.apple.com/101555. URLs like `fcs-keys-pub-prod.cdn-apple.com`, `wkms-public.apple.com`, `gs.apple.com`, etc, are all required to be whitelisted in your firewall/proxy to create macOS VMs.
  - If attempting to set/use `http_proxy` or `https_proxy`, they will not work. There is a requirement for full internet access to set up macOS properly in later 15.x VM versions (you'll see `status 70` errors if this is the case). You'll need to use `ANKA_NETWORK_MODE=disconnected` when creating the VM and then use `anka modify {VM} network --mode shared` after creation to enable networking again.
4. The hardware you have will work with the specific OS you're running on it. Apple has limited the ability to install macOS on specific hardware models. This differs for each major version of macOS. <a href="https://support.apple.com/kb/index?q=is+compatible+with+these+computers&src=globalnav_support&type=organic&page=search&locale=en_US">You can search for the supported hardware pages for each release here.</a>
5. **[ARM/Silicon]** Starting in 15.x, Apple requires that you create and run VMs on the same hardware type. For example, you cannot create on an M1 with 15.5 and then try to run it on an M2 or M4. Apple prevents this sort of cross-compatibility based on the CPU architecture and 15.x host OS. However, this does not get enabled if creating on 14.x host OS and then trying to run it on a 15.x host with a different CPU architecture.

---

## Create your first VM

You have two methods of creating your Anka VMs. We will describe both in this guide, but you only really need to choose one.

1. [With the **anka create** command](#using-anka-create) (recommended).
2. [With the Anka.app UI](#using-the-anka-gui).

---

### Supported VM macOS versions

Anka allows you to create VMs for the following macOS versions. There may be some limitations which will be included below the table.

{{< hint warning >}}
It's possible that this table is out of date and newer versions are supported. Please try creation on the version you want before reaching out to support. We will do our best to keep this table up to date.
{{< /hint >}}

{{< rawhtml >}}
<div style="display:flex; text-align: center;">
<div style="width: 50%">
<h4 style="padding: 10px;">Anka 3 (arm64/Silicon)</h4>
<table>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<tbody style="text-align:center;">
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.1 beta (25B5042k)<span style="cursor: help;" title="Requires setting a disk size of 50GB or more.
15.x host: Requires Xcode 26 or later on the host, fully configured.">&#8505;</span>
        <a href="https://updates.cdn-apple.com/2025FallSeed/fullrestores/082-91406/9745D030-E51F-497A-9E26-1B40A07FE9AE/UniversalMac_26.1_25B5042k_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
      </b>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 26.0 (25A354) <span style="cursor: help;" title="Requires setting a disk size of 50GB or more.
15.x host: Requires Xcode 26 or later on the host, fully configured.">&#8505;</span>
        <a href="https://updates.cdn-apple.com/2025FallFCS/fullrestores/093-37622/CE01FAB2-7F26-48EE-AEE4-5E57A7F6D8BB/UniversalMac_26.0_25A354_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
      </b>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 15.6.1 (24G90)
        <a href="https://updates.cdn-apple.com/2025SummerFCS/fullrestores/093-10809/CFD6DD38-DAF0-40DA-854F-31AAD1294C6F/UniversalMac_15.6.1_24G90_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
      </b>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle">
      <b>
        macOS 15.6 (24G84)
        <a href="https://updates.cdn-apple.com/2025SummerFCS/fullrestores/082-08674/51294E4D-A273-44BE-A280-A69FC347FB87/UniversalMac_15.6_24G84_Restore.ipsw" title="Download IPSW" style="margin-left: 8px; text-decoration: none;" target="_blank" rel="noopener">
          <span class="fa fa-download" style="font-size:1.2em;"></span>
        </a>
      </b>
    </td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.5 (24F74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4.1 (24E263)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 (24E248)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 beta (24E5206s)</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#10060;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 beta 3 (24E5228e)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 beta 3 (24D5055b)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 beta 2 (24E5222f)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 beta 2 (24D5040f)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.2 (24D81)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.1 (24D70)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 (24D60)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.2 (24C101)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B91)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B2091)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1 (24B2083)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1 (24B83)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.0.1 (24A348)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.0 (24A335)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.6.1 (23G93)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.6 (23G80)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.5 (23F79)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.4.1 (23E224)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.3.1 (23D60)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.4 (23E214)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.3 (23D56)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.2.1 (23C71)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.1.2 (23B92)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.2 (23C64)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.1 (23B74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.1.1 (23B81)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.6 (22G120)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.0 (23A344)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.5.1 (22G90)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.5.2 (22G91)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.5 (22G74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.4.1 (22F82)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.4 (22F66)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.3.1 (22E261)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.3 (22E252)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.2.1 (22D68)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.2 (22D49)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.1 (22C65)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.0.1 (22A400)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.0 (22A380)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.6.1 (21G217)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.6 (21G115)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.5.1 (21G83)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.5 (21G72)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.4 (21F79)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.4 (21F2081)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.3.1 (21E258)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.3 (21E230)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.2.1 (21D62)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.2 (21D49)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.0.1 (21A559)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.1 (21C52)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
</tbody>
</table>
</div>


<div style="width: 50%">
<h4 style="padding: 10px;">Anka 3 (amd64/intel)</h4>
<table>
<tbody style="text-align:center">
  <tr>
    <td style="vertical-align: middle"><b>macOS 26.0 (25A354)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.6.1 (24G90)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.6 (24G84)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.5 (24F74)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4.1 (24E263)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.4 (24E248)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.2 (24D2082)</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#10060;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.2 (24D81)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3.1 (24D70)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.3 (24D60)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.2 (24C101)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B91)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 15.1.1 (24B2091)</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#10060;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.7.4 (23H420)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.7.3 (23H417)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 14.7.2 (23H311)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.7.4 (22H420)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.7.3 (22H417)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.7.2 (22H313)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.7.4 (21H1123)</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 11.7.10 (20G1427)</b></td>
    <td style="font-size: 1.5rem; background-color:rgb(238, 179, 18);">&#9989;</td>
  </tr>
</tbody>
</table>
</div>
</div>
{{< /rawhtml >}}

### Specific Requirements

- **[Intel]** 10.13/14 VM creation requires installing a kext (https://github.com/pmj/virtio-net-osx) on the VM for networking to function.
- **[ARM]** Creation of later 15.x and any 26.x VMs requires you to prepare the host. The following steps are required by Apple:
  - Install Xcode 26 or later and set it up fully with the following commands:
      ```bash
      XCODE_DESTINATION="/Applications"
      XCODE_APP="Xcode.app"
      sudo /usr/sbin/dseditgroup -o edit -a everyone -t group _developer
      sudo xcode-select -s ${XCODE_DESTINATION}/${XCODE_APP}/Contents/Developer
      sudo xcodebuild -license accept
      sudo xcodebuild -runFirstLaunch
      sudo DevToolsSecurity -enable
      for PKG in $(/bin/ls ${XCODE_DESTINATION}/${XCODE_APP}/Contents/Resources/Packages/*.pkg); do
          sudo /usr/sbin/installer -pkg "$PKG" -target /
      done
      echo "Checking Xcode CLI tools"
      sudo xcode-select -s "${XCODE_DESTINATION}/${XCODE_APP}"
      xcode-select -p &> /dev/null
      if [ $? -ne 0 ]; then
        echo "Xcode CLI tools not found. Installing them..."
        touch /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress;
        PROD=$(softwareupdate -l |
          grep "\*.*Command Line" |
          tail -n 1 | sed 's/^[^C]* //')
        softwareupdate -i "$PROD" --verbose;
      else
        echo "Xcode CLI tools OK"
      fi
      ```
  - Install the Metal Toolchain or else overall performance of macOS 26 VMs will be poor (icons slow to load, etc)
      ```bash
      xcodebuild -downloadComponent metalToolchain
      xcodebuild -importComponent metalToolchain
      ```
- **[ARM]** To fully support macOS 14.x VMs, you must have macOS 14.x (or higher) on your host. Similarly, running 13.x VMs also require a minimum host macOS version of 13.x.
- **[ARM]** Creation of 15.x VMs on 14.x hosts requires Xcode 16.2 OR the MobileDevice.pkg (inside of Xcode.app) is installed.
- **[ARM]** There is also a rare problem where your Xcode is not fully set up and still creates problems. Be sure to run the following:
  ```bash
  sudo xcodebuild -license accept
  sudo xcodebuild -runFirstLaunch
  for PKG in $(/bin/ls /Applications/Xcode.app/Contents/Resources/Packages/*.pkg); do
      sudo /usr/sbin/installer -pkg "$PKG" -target /
  done
  ```

<!-- {{< hint warning >}}
VM creation requires a full internet connection or access to the Apple URLs detailed on https://support.apple.com/101555. If you are behind a corporate firewall/proxy, you'll need to set up a proxy that has the access you need and tell the system to use that proxy with the following:

```bash
# networksetup is an alternative to going into the System Preferences > Network > <interface> > Details > Proxies and setting it up manually
networksetup -setwebproxy <interface> <proxyURL> <port>
networksetup -setsecurewebproxy <interface> <proxyURL> <port>
export http_proxy=http://<proxyURL>:<port>
export https_proxy=http://<proxyURL>:<port>
```

If this does not work, you can try setting the `ANKA_NETWORK_MODE` environment variable to `disconnected` to temporarily disable networking entirely and eliminate the requirement.
{{< /hint >}} -->

---

### Using `anka create`

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_example.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/create/_extra.md" >}}

After executing `anka create`, Anka will automatically set up macOS, create the user `anka` with password: `admin`, disable SIP, and enable VNC for you. The VM will then be stopped.

---

### Using the Anka GUI

1. Click on **Create new VM**.
2. **LEAVE INSTALLER BLANK** and click on Options to set any non-default values you want.
  {{< hint info >}}
  Leaving the installer blank will automatically target the latest macOS version, pulling the IPSW file from the official Apple CDN (`updates.cdn-apple.com`). You can use your own IPSW file with the Anka CLI instead.
  {{< /hint >}}
![installer with pkg]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/ui.png)
3. Be patient while it's creating.

Once the VM is created, you will see it on the sidebar -- Hooray!

![ui with vm in the sidebar list]({{< siteurl >}}images/getting-started/creating-your-first-vm/ui-vm-in-sidebar.png)

#### Set up the VM (post-GUI creation)

{{< hint warning >}}
This is not needed if you ran `anka create`.
{{< /hint >}}

The GUI tool will not automatically set up macOS and requires you to perform several steps manually.

1. Start the VM with `anka start -uv` to launch the [Anka Viewer]({{< relref "anka-virtualization-cli/command-line-reference.md#view" >}}) and addons. **Please do not use `-uv` in your scripts when starting VMs. It is only for interactive/logged in sessions.**

- `anka view` does not currently work post-start unless you started it with -v.
- **ARM USERS:** `sudo anka view` as a normal user is not possible yet. You'll need to ensure that VNC is enabled to access VMs running under `sudo`.

2. Once inside the Anka Viewer/VM, finish the macOS installation **and be sure to install the addons package through the disk we mounted with `-u`**.

3. After you're finished, reboot the VM.

![mounted addons]({{< siteurl >}}images/apple/getting-started/creating-your-first-vm/addonspkg.png)

{{< hint warning >}}
**For our addons to install and enable autologin properly, you need to create the VM user as username: `anka` and password: `admin`. If you decide to use your own username and password, you will need to manually enable autologin for the user.**
{{< /hint >}}

#### Disable SIP in Recovery Mode (post-GUI creation)

{{< hint info >}}
You can start the VM in Recovery Mode with `ANKA_START_MODE=2`:

```bash
ANKA_START_MODE=2 anka start 12.6
```

{{< /hint >}}

{{< hint warning >}}
SIP is only enabled for VMs created in the Anka App's UI. These instructions are irrelevant for VMs created with `anka create`.
{{< /hint >}}

With SIP enabled, there are two main issues you'll find when running VMs:

1. User or command executions can hang due to a "allowed to access" dialog in the VM's UI. It requires VNC access and manual intervention to get around (no commands to disable this protection feature).
2. Apple's `syspolicyd` will notice applications and processes running for the first time and consume a lot of CPU and RAM trying to scan them.

In order to disable SIP, you need to first launch the VM in recovery mode.

![recovery-mode]({{< siteurl >}}images/apple/getting-started/accessing-your-vm/anka-app-recovery-mode.png)

Then, you launch the Terminal application and execute `csrutil disable`. Once executed and after confirmation that the command worked, you can stop the VM. On next boot, SIP will be disabled.

---

## Optimizing your VM

It's recommended that you disable:

- Spotlight and `coreduetd`:

```bash
# Disable indexing volumes
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes" || true
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network" || true
sudo killall mds || true
sleep 60
sudo mdutil -a -i off / || true
sudo mdutil -a -i off || true
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist || true
sudo rm -rf /.Spotlight-V100/*
rm -rf ~/Library/Metadata/CoreSpotlight/ || true
killall -KILL Spotlight spotlightd mds || true
sudo rm -rf /System/Volums/Data/.Spotlight-V100 || true
```

- SIP, as `syspolicyd` will scan running processes and slow everything down.

---

## Listing available VMs in the CLI

{{< include file="_partials/anka-virtualization-cli/command-line-reference/list/_example.md" >}}


## Stop or Suspend the VM

{{< hint info >}}
This is not necessary for `anka modify` commands.
{{< /hint >}}

Once you've finalized your changes inside of the VM, be sure to use `anka stop` or `anka suspend`.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/stop/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/suspend/_index.md" >}}

{{< include file="_partials/anka-virtualization-cli/command-line-reference/suspend/_extra.md" >}}

## Deleting a VM

### Anka CLI

```shell
❯ anka delete test
are you sure you want to delete vm 77f33f4a-75c3-47aa-b3f6-b99e7cdac001 test [y/N]:
```

### Anka GUI

![edit menu delete]({{< siteurl >}}images/getting-started/creating-your-first-vm/edit-menu-delete.png)

---

## VM Clones

### Disk Optimization

Customers coming from Anka 2 will know that when you clone a VM (_untagged_ or _tagged_), it will share the underlying VM image files between the two. However, this is not the case for Anka 3. As of right now, sharing of the underlying VM image files between a clone and its source requires first creating a tag for the source _before_ you clone. You can do this with `anka push --local`, or just a regular `anka push` if you're running the [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}). Don't worry, clones will not have access to change the original source VM state.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/push/_index.md" >}}

```bash
❯ anka list
+--------+--------------------------------------+----------------------+---------+
| name   | uuid                                 | creation_date        | status  |
+--------+--------------------------------------+----------------------+---------+
| 12.0.1 | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+--------+--------------------------------------+----------------------+---------+

❯ anka push --local --tag vanilla 12.0.1

❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
```

_The above example shows the tag "vanilla" does not exist locally until we execute the `anka push --local`._

{{< hint info >}}
Cloned VMs will use a trivial amount of disk space until you start them. Once started, an empty image is created and connected on top of existing images and any changes to or in macOS are then added to it.
{{< /hint >}}

{{< hint info >}}
To switch between tags locally, you can use the `anka pull --local --tag {targetTagname} {VMName}` command:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/pull/_index.md" >}}

{{< /hint >}}

### Cloning

You can easily create VM clones from a source VM and _its current state_ using `anka clone`:

{{< include file="_partials/anka-virtualization-cli/command-line-reference/clone/_index.md" >}}

{{< hint info >}}
Don’t worry, at no point do clones have access to change the original source VM state.
{{< /hint >}}

{{< hint info >}}
All clones share as many underlying layers and data as possible.
{{< /hint >}}

There are two types of cloning you can perform:

1. **Shallow Clone**: `anka clone {source} {dest}`
    - Shallow clones allow you to get a new VM with a new name and UUID. Underlying layers are shared with the source VM (if the source VM has a tag). When the shallow clone VM is started, a new layer for that specific clone is created on top of existing layers that make up the VM.
2. **Full Clone**: `anka clone --copy {source} {dest}`
    - Full clones create a copy that merges all underlying layers so that they cannot be shared with other VMs. While this could theoretically shrink the size of the VM, it loses the ability to re-use existing layers from other VMs on the host and can actually use more disk space than before.

```bash
❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+


❯ anka clone 12.0.1 12.0.1-xcode13
6070ee59-6c16-4c93-ba7a-122b66b1472a

❯ anka list
+------------------+--------------------------------------+----------------------+---------+
| name             | uuid                                 | creation_date        | status  |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1 (vanilla) | 002b73b6-dc99-4d6b-8f68-6067a3a66d73 | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
| 12.0.1-xcode13   | 6070ee59-6c16-4c93-ba7a-122b66b1472a | Nov 19 08:02:33 2021 | stopped |
+------------------+--------------------------------------+----------------------+---------+
```

### VM Templates

{{< include file="_partials/anka-build-cloud/vm-templates.md" >}}

## Exporting and Importing VMs

### Export
{{< include file="_partials/anka-virtualization-cli/command-line-reference/export/_extra.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/export/_index.md" >}}

### Import
{{< include file="_partials/anka-virtualization-cli/command-line-reference/import/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/import/_extra.md" >}}

---

## Anka Build Cloud

### Add the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

### (Anka Build Cloud only) Push the VM to the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

---

## What's next?

- [Acessing your VM]({{< relref "anka-virtualization-cli/getting-started/accessing-your-vm.md" >}})
