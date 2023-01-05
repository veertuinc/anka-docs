---
---

{{< hint error >}}
EOL and Deprecation list:
- Anka Virtualization + CLI < 2.0 EOL
{{< /hint >}}

{{< hint info >}}
We recommend using our diagnostic package collection script. This script collects logs and usage statistics from the host and archives them.

```bash
curl https://raw.githubusercontent.com/veertuinc/scripts/main/generate-anka-node-diagnostics.bash -O && \
chmod +x generate-anka-node-diagnostics.bash && \
./generate-anka-node-diagnostics.bash
```

It is also best to run this as close to when an issue happens as several commands grab a window in time.
{{< /hint >}}

**Once you have collected the logs, go through the below mentioned troubleshooting steps and then contact support.**

---

## Anka Virtualization

### Troubleshooting Checklist

- Ensure that you've run `sudo anka license accept-eula` on the host.
- Make sure you've got a valid license with `sudo anka license validate`.
- Check `df -h` and other host or VM resource usage (CPU/RAM) and be sure that the host resources are not exhausted.
- Ensure that the user running Anka VMs is logged in and that Sleep and passworded Screensaver are disable. The hypervisor will not work without an active/logged in user.
- Check `[~]/Library/Logs/Anka/` logs for any indication of why the failure happened.
- Ensure that all components in your Anka environment can communicate. This includes connectivity between CI/CD tooling we do not support.
- Disable anti-virus and firewalls on the host.

### Logs

{{< hint warning >}}
Anka runs as any user on the host, so the paths and locations described below can sometimes be under `$HOME/~`.
{{< /hint >}}

- Anka CLI commands executed as root/sudo: **`/Library/logs/anka`**
- Anka CLI commands as a non-root user: **`$HOME/Library/logs/anka`**

{{< hint info >}}
The above logs are rotated at 1MB and maintains a maximum of 10 files. This is not configurable.
{{< /hint >}}

In these directories you will find the following logs:

1. `anka.log` - The primary log including anka commands (Anka CLI, Anka run) STDOUT and STDERR

2. `lupd.log` - License auto-upgrade service logs; it's rarely used and most likely not related to VMs runtime

3. `ankanetd.log` - Anka VM network service logs; useful for troubleshooting VM networking related problems

4. `{UUID}.log` - (`{UUID}` is the specific VM's UUID) Specific VM logs; useful if VM exits abnormally or fails to start.

{{< hint info >}}
If you're using the Anka Build Cloud Controller to start and terminate VMs, you need to set an ENV so the Node Agent doesn't delete the `{UUID}.log` on termination. This requires editing the agent's plist:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>EnvironmentVariables</key>
    <dict>
        <key>ANKA_DELETE_LOGS</key>
        <string>0</string>
    </dict>
    <key>Label</key>
    <string>com.veertu.anka.cluster.agent</string>
    <key>ProgramArguments</key>
    <array>
      <string>/Library/Application Support/Veertu/Anka/bin/anka_agent_helper</string>
      <string>--controller-addr</string>
```
{{< /hint >}}

5. `/var/log/install.log` - Install related STDOUT/ERR

Anka and Apple will also report crashes in `/Library/Logs/DiagnosticReports` as `.crash` or `.hang`.

---

## Build Cloud (optional)

{{< hint warning >}}
Only relevant if you've [joined your Node to the Build Cloud Controller & Registry]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).
{{< /hint >}}

{{< include file="_partials/troubleshooting/node-agent.md" >}}

{{< include file="_partials/troubleshooting/centralized-logs.md" >}}
