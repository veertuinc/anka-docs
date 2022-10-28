---
---

Anka VM Templates support the following macOS versions:

| Version Number | Version Name |
| --- | --- |
| **13.x** | macOS Ventura |
| **12.x** | macOS Monterey |

{{< hint warning >}}
Creating 13.x VMs on Monterey (12.x) requires that Xcode >= 14.0.1 is installed. This is a requirement from Apple at the moment.

There is also a rare problem where your Xcode is not fully set up and still creates problems, regardless of being on Ventura. Be sure to run the following:

```bash
sudo xcodebuild -license accept
sudo xcodebuild -runFirstLaunch
for PKG in $(/bin/ls /Applications/Xcode.app/Contents/Resources/Packages/*.pkg); do
    sudo /usr/sbin/installer -pkg "$PKG" -target /
done
```
{{< /hint >}}
