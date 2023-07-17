---
date: 2019-12-12T00:00:00-00:00
title: "Installing Anka"
linkTitle: "Installing Anka"
weight: 1
description: >
  How to install the Anka Virtualization software on your macOS
---

## MacOS Host Version Support

{{< rawhtml >}}
<div style="display:flex;">
<table>
<tbody style="text-align:center">
  <tr>
    <td style="font-size: 1.5rem; background-color: #f2e6ff;"><b>INTEL</b></td>
    <td style="background-color: #f2e6ff;"><b>Anka 3.3.x</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 10.14</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 10.15</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;" class="emojifont">⚠️</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 11.x</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;" class="emojifont">⚠️</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.x</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;" class="emojifont">⚠️</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.x</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
</tbody>
</table>
<table>
<tbody style="text-align:center;">
  <tr>
    <td style="font-size: 1.4rem; background-color: #ccebff;"><b>Apple/ARM</b></td>
    <td style="background-color: #ccebff;"><b>Anka 3.3.x</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;" class="emojifont">⚠️</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
</tbody>
</table>
</div>
{{< /rawhtml >}}

{{< hint warning >}}
<b class="emojifont">⚠️</b> We use several APIs that are only available starting in macOS 12.3 and later. While Anka may install, and work, there are features that will cause it to crash. Please upgrade macOS to the latest before using Anka.
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
```

---

## Verifying the installation

```shell
❯ anka version            
Anka Build Basic version 3.X.X (build XXX)
```

---

## What's next?

- [Creating your first VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})
