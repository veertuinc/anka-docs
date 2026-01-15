---
title: "Using Local Storage"
linkTitle: "Using Local Storage"
weight: 1
description: >
  Use local storage disk for AWS EC2 Mac instances
---

According to https://aws.amazon.com/blogs/aws/announcing-amazon-ec2-m4-and-m4-pro-mac-instances/, M4 instances now come with a local storage disk. This guide will walk you through how to prepare it in your own AMIs.

### Step 1: Log into the instance

```bash
ssh -i ~/.ssh/your-key.pem ec2-user@your-instance-public-ip
```

### Step 2: Set password and enable VNC

Within the terminal in VNC or SSH, run the following commands:

```bash
sudo passwd ec2-user
sudo launchctl enable system/com.apple.screensharing
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.screensharing.plist
```

### Step 3: Log in with VNC and enable Full Disk Access and Automatic Login

After connecting the screen sharing session, run the `open` commands to quickly jump to the System Settings panes:

```bash
open "x-apple.systempreferences:com.apple.settings.PrivacySecurity?Privacy_AllFiles"
```

**Hint:** In the "Add" dialog you could use `⌘⇧G` to quickly navigate to the app or files.

```bash
/Applications/Anka.app
/Library/Application Support/Veertu/Anka/bin/anka_agent
/Library/Application Support/Veertu/Anka/bin/ankacluster
```

Finally, execute `open "x-apple.systempreferences:com.apple.preferences.users"` to enable Automatic Login for the current user.

### Step 4: Prepare the local storage disk

We provide a script to prepare the local storage disk for use in Anka. You can find it here: https://github.com/veertuinc/aws-ec2-mac-amis/blob/main/prepare-local-disk.bash

It's included in our AMIs, so you can just run the following command:

```bash
cd ~/aws-ec2-mac-amis
git pull
sudo ./prepare-local-disk.bash
```

Once it runs, the local ec2-user and root user will both point to `/Volumes/Anka` for their VM storage directories.

If you are using Port Forwarding on your VMs, it's important to confirm the `Allow ankahv-arm64 to find devices on local networks` prompt while you have VNC open. To do this, you need to:

1. Pull a VM with port forwarding enabled, or, create a new one then use the modify commands to add port forwarding.
2. Start the VM and inside of VNC you should see a prompt to allow the VM to find devices on local networks. Click "Allow".
3. Clean up the VM from the host with anka delete (and delete any ipsw files it downloaded into `/Volumes/Anka/*/*.ipsw` if you ran `anka create`).

### Step 5: Cleanup

You can now disable VNC:

```bash
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.screensharing.plist
sudo launchctl disable system/com.apple.screensharing
```