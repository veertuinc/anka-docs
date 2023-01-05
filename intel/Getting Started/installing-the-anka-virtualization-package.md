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
<table>
<tbody style="text-align:center">
  <tr>
    <td></td>
    <td><b>Anka 2.4.x</b></td>
    <td><b>Anka 2.5.x</b></td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 10.14</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 10.15</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 11.x</b></td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 12.x</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
  <tr>
    <td style="vertical-align: middle"><b>macOS 13.x</b></td>
    <td style="font-size: 1.5rem; background-color: #c0392b;">&#128721;</td>
    <td style="font-size: 1.5rem; background-color: #2ecc71;">&#9989;</td>
  </tr>
</tbody>
</table>
{{< /rawhtml >}}

## UI download and install

{{< include file="_partials/intel/anka-virtualization-cli/_download-and-install.md" >}}

---

## Terminal download and install

{{< include file="_partials/intel/anka-virtualization-cli/_download-and-install-with-terminal.md" >}}

---

## Verify the installation

{{< include file="_partials/intel/anka-virtualization-cli/_verify-installation.md" >}}

---

## Accept the EULA

```bash
sudo anka license accept-eula
```

---

## What's next?

- [Creating your first VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}})
