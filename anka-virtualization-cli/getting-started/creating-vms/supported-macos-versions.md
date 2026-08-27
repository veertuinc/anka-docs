---
title: "Supported macOS versions for VM creation"
linkTitle: "Supported macOS versions"
weight: 1
description: >
  Compatibility matrix and host requirements for anka create
aliases:
  - "/anka-virtualization-cli/getting-started/supported-macos-versions/"
---

Use this page to check whether you can create a given **guest** macOS version on your **host** (ARM or Intel). For the creation steps themselves, see [Creating VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms/_index.md" >}}).

{{< hint warning >}}
This table can lag behind the latest Apple releases. Try `anka create --list` on your host before contacting support.
{{< /hint >}}

## How to read the tables

| Symbol | Meaning |
|--------|---------|
| ✅ | Creation is supported for this guest version on Anka 3 |
| ❌ | Not supported (often a specific beta build) |

- **ARM (Silicon):** Click the ℹ️ button next to a version for Xcode, MobileDevice.pkg, or disk size requirements.
- **Intel:** Requirements are listed in the table where applicable; see also [Specific host requirements](#specific-host-requirements) below.

{{< include file="_partials/anka-virtualization-cli/getting-started/_vm-version-tables.md" >}}

## Specific host requirements

### ARM (Silicon)

Creation of later 15.x and any 26.x guests requires host preparation mandated by Apple:

- Install **Xcode 26 or later** and complete setup:

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

- Install the **Metal Toolchain** or macOS 26 guest performance will suffer (slow icons, and similar):

```bash
xcodebuild -downloadComponent metalToolchain
xcodebuild -importComponent metalToolchain
```

- On an **older host OS**, install the latest **MobileDevice.pkg** from the Xcode version that matches the guest you want. Example: on host macOS 15.x, to create 26.4 guests, install MobileDevice.pkg from Xcode 26.4 (`Xcode.app/Contents/Resources/Packages/`). You may need a beta Xcode to obtain that package. We try to upload the latest MobileDevice.pkg in our https://downloads.veertu.com/#anka/ page, but, if it's not there, it may be available on the developer.apple.com downloads site. If it's not there, that usually means that Apple has included it in the latest macOS release or stable Xcode version and a individual package is not needed.

- If creation still fails, re-run package install and license acceptance:

```bash
sudo xcodebuild -license accept
sudo xcodebuild -runFirstLaunch
for PKG in $(/bin/ls /Applications/Xcode.app/Contents/Resources/Packages/*.pkg); do
    sudo /usr/sbin/installer -pkg "$PKG" -target /
done
```
