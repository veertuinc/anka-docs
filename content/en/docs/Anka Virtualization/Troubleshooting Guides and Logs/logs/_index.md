---
title: "Logs"
linkTitle: "Logs"
weight: 2
description: >
---

## OVERVIEW

#### Default Log Location
- Anka CLI commands executed as root/sudo: **`/Library/logs/anka`**
- Anka CLI commands as a non-root user: **`$HOME/Library/logs/anka`**

> The above logs are rotated at 1MB and maintains a maximum of 10 files. This is not configurable.

In these directories you will find the following logs:

1. `anka.log` - The primary log including anka commands (Anka CLI, Anka run) STDOUT and STDERR

2. `lupd.log` - License auto-upgrade service logs; it's rarely used and most likely not related to VMs runtime

3. `ankanetd.log` - Anka VM network service logs; useful for troubleshooting VM networking related problems

4. `{UUID}.log` - (`{UUID}` is the specific VM's UUID) Specific VM logs; useful if VM exits abnormally or fails to start. **These files are deleted once the VM is terminated**

    > If you're using the Anka Build Cloud Controller to start and terminate VMs, you need to set an ENV so the Node Agent doesn't delete the `{UUID}.log` on termination. This requires editing the agent's plist:
    >    ```xml
    >    <?xml version="1.0" encoding="UTF-8"?>
    >    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    >    <plist version="1.0">
    >    <dict>
    >        <key>EnvironmentVariables</key>
    >        <dict>
    >            <key>ANKA_DELETE_LOGS</key>
    >            <string>0</string>
    >        </dict>
    >        <key>Label</key>
    >        <string>com.veertu.anka.cluster.agent</string>
    >        <key>ProgramArguments</key>
    >        <array>
    >          <string>/Library/Application Support/Veertu/Anka/bin/anka_agent_helper</string>
    >          <string>--controller-addr</string>
    >    ```

5. `/var/log/install.log` - Install related STDOUT/ERR