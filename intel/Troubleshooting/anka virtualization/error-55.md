---
title: "VM log filled with error 55 / VM packet loss"
linkTitle: "VM log filled with error 55 / VM packet loss"
weight: 1
---

## Scenarios

- The `[~]/Library/Logs/Anka/{UUID}.log` or `ankanetd.log` is filled with `22: failed to write packet, error 55` and `failed to send # packets: error 55`
- Sudden packet loss inside of the VM or to the VM.

## Solution

This is very common when the VMs have heavy CPU usage. Normally it shouldn't be a critical error -- packet loss is being handled by the TCP stack of guest. But if the error rate is very high, it definitely could cause applications or downloads to fail inside of the VM or even communication to the VM to be severed. If the occurrence of these errors is more than just a few log entries, you can increase the value of `net.local.dgram.recvspace` and `net.local.dgram.maxdgram` with sysctl.

{{< hint info >}}
You may need to keep increasing the value until the errors are no longer visible.
{{< /hint >}}

```bash
$ sudo sysctl -a | grep "local.dgram"
net.local.dgram.maxdgram: 2048
net.local.dgram.recvspace: 4096
```

```bash
sudo sysctl net.local.dgram.recvspace=65536
sudo sysctl net.local.dgram.maxdgram=16384
```

{{< hint warning >}}
Manually executed `sysctl` changes do not persist across reboots. You can setup a plist under `/Library/LaunchDaemons` to have it set on each boot.
```bash
sudo echo -n '
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>Label</key>
        <string>custom.net.local.dgram.recvspace</string>
        <key>ProgramArguments</key>
        <array>
        <string>/usr/sbin/sysctl</string>
        <string>net.local.dgram.recvspace=65536</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
</dict>
</plist>' > /Library/LaunchDaemons/custom.net.local.dgram.recvspace.plist
sudo launchctl load -w /Library/LaunchDaemons/custom.net.local.dgram.recvspace.plist
sudo echo -n '
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>Label</key>
        <string>custom.net.local.dgram.maxdgram</string>
        <key>ProgramArguments</key>
        <array>
        <string>/usr/sbin/sysctl</string>
        <string>net.local.dgram.maxdgram=16384</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
</dict>
</plist>' > /Library/LaunchDaemons/custom.net.local.dgram.maxdgram.plist
sudo launchctl load -w /Library/LaunchDaemons/custom.net.local.dgram.maxdgram.plist
```
{{< /hint >}}

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
