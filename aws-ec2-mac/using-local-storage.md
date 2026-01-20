---
title: "Using Local Storage"
linkTitle: "Using Local Storage"
weight: 3
description: >
  How to set up the local storage disk on your own AWS EC2 Mac AMI
---



According to https://aws.amazon.com/blogs/aws/announcing-amazon-ec2-m4-and-m4-pro-mac-instances/, AWS EC2 Mac M4 instances now come with a local storage disk. This significantly improves the performance of Anka VMs running on AWS EC2 Mac instances. This guide will walk you through how to prepare an AMI with the local storage disk.

Note: You'll want to start an instance from our official AMIs. This gives you all of the Anka specific tweaks.

{{< hint info >}}
Our AMIs do not ever set a password or enable VNC for security reasons, so you'll need to do this in your own AMI.
{{< /hint >}}

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

#### Enable Automatic Login

Finally, execute `open "x-apple.systempreferences:com.apple.preferences.users"` to enable Automatic Login for the current user.

### Step 4: Prepare the local storage disk

We provide a script to prepare the local storage disk for use in Anka. You can find it here: https://github.com/veertuinc/aws-ec2-mac-amis/blob/main/prepare-local-disk.bash

It's included in our AMIs, so you can just run the following command:

```bash
cd ~/aws-ec2-mac-amis
git pull
sudo ./prepare-local-disk.bash
```

It also will do the following:

- Unload the amazon instance storage disk mounter service to avoid conflicts with the local storage disk.
- The local `ec2-user` and `root` user will both point to `/Volumes/Anka` for their VM storage directories using `anka config` to set the storage locations.
- Disable indexing for the `/Volumes/Anka` volume with `sudo mdutil -a -i off /Volumes/Anka`

#### Prepare ankahv-arm64 to find devices on local networks (ec2-user only)

{{< hint warning >}}
This is only necessary for the `ec2-user` user. Processes running as `root` do not need to do this because they have full access already.

**ALSO:** It does NOT persist between instance creation using the final AMI. This is only useful for initial testing as the `ec2-user`. When you create a new instance from the eventual AMI, you'll see a prompt again when you try to start a VM with port forwarding as the `ec2-user`.
{{< /hint >}}

If you are using Port Forwarding on your VMs as the `ec2-user`, you'll need to confirm the `Allow ankahv-arm64 to find devices on local networks` prompt while you have VNC open. To do this, you need to:

1. Pull a VM with port forwarding enabled, or, create a new one then use the modify commands to add port forwarding.
2. Start the VM and inside of VNC you should see a prompt to allow the VM to find devices on local networks. Click "Allow".
3. Clean up the VM from the host with anka delete (and delete any ipsw files it downloaded into `/Volumes/Anka/*/*.ipsw` if you ran `anka create`).

### Step 5: Cleanup

You can now disable VNC:

```bash
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.screensharing.plist
sudo launchctl disable system/com.apple.screensharing
```

## FAQs

- If you Stop and Start the Instance, it will lose everything in the external drive and `/Volumes/Anka` directory. On start, the `prepare-local-disk.bash` script will automatically run and remount the local storage disk for you. However, normal inner OS reboots are not subject to this.
- Amazon is automounting the external drive named ephemeral0 with `/Library/LaunchDaemons/com.amazon.ec2.instance-storage-disk-mounter.plist` / `/usr/local/libexec/mount_instance_storage_disk.sh`.

---

A special thank you to Gregor for his help with this guide. It's customers like him that make Anka the best virtualization solution for macOS.