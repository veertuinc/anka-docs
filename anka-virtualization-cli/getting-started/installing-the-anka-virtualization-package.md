---
date: 2019-12-12T00:00:00-00:00
title: "Installing Anka"
linkTitle: "Installing Anka"
weight: 1
description: >
  How to install the Anka Virtualization software on your macOS
aliases:
  - "/apple/getting-started/installing-the-anka-virtualization-package/"
  - "/intel/getting-started/installing-the-anka-virtualization-package/"
---

## MacOS Host Version Support

{{< rawhtml >}}
<div class="compat-matrix-wrapper">
<table class="compat-matrix">
  <thead>
    <tr>
      <th scope="col">macOS host</th>
      <th scope="col" class="compat-matrix__head-intel">Intel<br>Anka 3.x</th>
      <th scope="col" class="compat-matrix__head-arm">Apple/ARM<br>Anka 3.x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">macOS 10.14</th>
      <td class="compat-matrix__cell compat-matrix__cell--unsupported" title="Not supported">&#128721;</td>
      <td class="compat-matrix__cell compat-matrix__cell--unsupported" title="Not supported">&#128721;</td>
    </tr>
    <tr>
      <th scope="row">macOS 10.15</th>
      <td class="compat-matrix__cell compat-matrix__cell--partial emojifont" title="Supported with limitations">&#128721;</td>
      <td class="compat-matrix__cell compat-matrix__cell--unsupported" title="Not supported">&#128721;</td>
    </tr>
    <tr>
      <th scope="row">macOS 11.x</th>
      <td class="compat-matrix__cell compat-matrix__cell--partial emojifont" title="Supported with limitations">&#128721;</td>
      <td class="compat-matrix__cell compat-matrix__cell--unsupported" title="Not supported">&#128721;</td>
    </tr>
    <tr>
      <th scope="row">macOS 12.x</th>
      <td class="compat-matrix__cell compat-matrix__cell--partial emojifont" title="Supported with limitations">&#128721;</td>
      <td class="compat-matrix__cell compat-matrix__cell--partial emojifont" title="Supported with limitations">&#128721;</td>
    </tr>
    <tr>
      <th scope="row">macOS 13.x</th>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;</td>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;</td>
    </tr>
    <tr>
      <th scope="row">macOS 14.x</th>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;</td>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;</td>
    </tr>
    <tr>
      <th scope="row">macOS 15.x</th>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;</td>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;<span class="compat-matrix__footnote-mark" title="See note below regarding local network prompt">**</span></td>
    </tr>
    <tr>
      <th scope="row">macOS 26.x</th>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;</td>
      <td class="compat-matrix__cell compat-matrix__cell--supported" title="Supported">&#9989;<span class="compat-matrix__footnote-mark" title="See note below regarding local network prompt">**</span></td>
    </tr>
  </tbody>
</table>
</div>
{{< /rawhtml >}}

{{< hint warning >}}
{{<rawhtml>}}
<b>**</b> Connecting to the VM over port forwarding for the first time will show a prompt on the host's desktop saying `"Allow ankahv-arm64 to find devices on local networks"`. You will need to manually approve this until a solution is provided by Apple.
{{< /rawhtml >}}
{{< /hint >}}

{{< hint warning >}}
<b class="emojifont">⚠️</b> MacOS 12.x is currently not supported for 3.3.9 and above. Please upgrade macOS to the latest before using Anka.
{{< /hint >}}

## Download and Install

### With GUI

1. Download the [universal](https://veertu.com/downloads/anka-virtualization-latest) PKG.
2. Once downloaded, double click the .pkg to start the installation process.

![installer with pkg]({{< siteurl >}}images/anka-virtualization/installer.png)

### With Terminal

![terminal installation]({{< siteurl >}}images/anka-virtualization/terminal-installation.png)

#### Apple/ARM & Intel

```shell
FULL_FILE_NAME="$(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/anka-virtualization-latest | cut -d/ -f5)"
curl -S -L -o ./$FULL_FILE_NAME https://veertu.com/downloads/anka-virtualization-latest
sudo installer -pkg $FULL_FILE_NAME -tgt /

# Install the device support package (15.4 support)
curl -S -L -o ./DeviceSupport-15.4.pkg https://downloads.veertu.com/anka/DeviceSupport-15.4.pkg
sudo installer -pkg ./DeviceSupport-15.4.pkg -tgt /
```

---

## Verifying the installation

```shell
❯ anka version            
Anka Build Basic version 3.X.X (build XXX)
```

---

## Activate your Anka Build Cloud License

[Details about our licensing]({{< relref "licensing.md" >}})


{{< include file="_partials/Licensing/activating.md" >}}

---

## What's next?

- [Creating your first VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})
