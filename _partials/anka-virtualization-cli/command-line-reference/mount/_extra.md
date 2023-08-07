---
---

{{< hint warning >}}
**Deprecated, please mount a network volume inside of the VM instead.**

1. Ensure **File Sharing** is on for the Host (System Preferences > Sharing)
2. Click on **(i)** next to the File Sharing toggle button. Set up your **Shared Folders** to be mounted in the VM. In this example, I'll mount my host's home directory which is named `veertu`.
3. I'll then go under **Options...** and ensure the **On** checkbox is enabled to the left of the user I'm trying to log into the host with.
4. Now use `anka run` to mount the host share with SMB: `anka run {VM} bash -c "mkdir -p ~/HOST; mount -t smbfs '//veertu[:password]@192.168.64.1/veertu' ~/HOST"` (we provide 192.168.64.1 inside of the VM which routes to the host)

I can then access the host user's home directory (`/Users/veertu`) inside of the VM at `~/HOST`.

```bash
❯ anka run test bash -c "df -h"
. . .
//veertu@192.168.64.1/veertu  932Gi  590Gi  342Gi    64% 154592960  89606492   63%   /Users/anka/HOST
```

{{< /hint >}}
