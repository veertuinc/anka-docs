---
title: "VM log filled with error 55 / VM packet loss"
linkTitle: "VM log filled with error 55 / VM packet loss"
weight: 1
---

## Scenarios

- The `[~]/Library/Logs/Anka/{UUID}.log` or `ankanetd.log` is filled with `22: failed to write packet, error 55` and `failed to send # packets: error 55`
- Sudden packet loss inside of the VM

## Solution

The send buffer is likely exceeding. Normally it shouldn't be a critical error -- packet loss is being handled by the TCP stack of guest. But if the error rate is very high, it definitely could be a symptom of network problems happening in guests. If the occurrence of these errors is more than just a few log entries (10s every millisecond for example), you need to increase the value of `sysctl net.local.dgram.recvspace`

```bash
sudo echo -n '
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>Label</key>
        <string>com.bitrise.net.local.dgram.recvspace</string>
        <key>ProgramArguments</key>
        <array>
        <string>/usr/sbin/sysctl</string>
        <string>net.local.dgram.recvspace=65536</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
</dict>
</plist>' > /Library/LaunchDaemons/com.veertu.anka.recvspace.plist
```

{{< hint warning >}}
You may need to keep increasing the value until the errors are no longer visible.
{{< /hint >}}

{{< hint warning >}}
Manually executed `sysctl` changes do not persist across reboots.
{{< /hint >}}

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
