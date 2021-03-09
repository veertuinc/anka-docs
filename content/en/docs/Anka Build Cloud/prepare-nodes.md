---
date: 2019-07-26
title: "Preparing your Anka Nodes"
linkTitle: "Preparing your Anka Nodes"
weight: 3
description: >
  Prepare high availability macOS machines before connecting to the Anka Build Cloud Controller
---

Once the Anka Build Virtualization software has been installed onto a macOS machine, you'll typically want to ensure that the machine has high availability. This requires turning off sleep and several other default features that would cause the machine to become unavailable. Below are the preparatory steps we suggest:

1. **You must log into your machine's administrator user** to ensure Apple's services are started.

2. **Enable automatic login for the current user:** Go to Preferences > Users > enable automatic login. Or, [using the CLI](https://github.com/veertuinc/kcpassword).

    > If using the CLI method, you must also XOR-encrypt the login password and add it to `/etc/kcpassword`.
    > The `/etc/kcpassword` file must be owned by `root:wheel` with a mode of `0600`.
    > See the GitHub repository [veertuinc/kcpassword](https://github.com/veertuinc/kcpassword) for help generating the encrypted password string.

3. **Disable require password after screensaver has started:** Go to Preferences > Security > under General > uncheck `require password after screensave or sleep begins` option.

    > VNC may be required to disable this. It's possible your hardware does not have VNC enabled and you also don't have physical access. The following are the commands necessary to enable VNC from an SSH:
    >  ```bash
    >  The following examples are only for OS X 10.5 and later.
    >  cd /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/
    >  sudo ./kickstart -configure -allowAccessFor -specifiedUsers
    >  sudo ./kickstart -configure -allowAccessFor -allUsers -privs -all
    >  sudo ./kickstart -activate
    >  ```

4. **Disable all forms of sleep:** Go to Preferences > Energy Saver > disable any sleep options.

    Alternatively, you can do this from the command-line:

    ```shell
    # Never go into computer sleep mode
    sudo systemsetup -setcomputersleep Off
    systemsetup -setcomputersleep Off || true
    # Disable standby
    sudo pmset -a standby 0
    # Disable disk sleep
    sudo pmset -a disksleep 0
    # Hibernate mode is a problem on some mac minis; best to just disable
    sudo pmset -a hibernatemode 0
    ```

5. **Disable spotlight:**

    If you cannot perform launchctl commands, you can execute these commands from the command-line:

    ```shell
    # Disable indexing volumes
    sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes"
    sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network"
    sudo killall mds
    sleep 60
    # Make sure indexing is DISABLED for the main volume
    sudo mdutil -a -i off /
    sudo mdutil -a -i off
    ```

    > MDS can be disable with `sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist`, but only if SIP is disabled on the host (not recommended)

6. **Big Sur Only (optional):** Disable Apple's mitigations with `sudo anka config vmx_mitigations 0`. Without it, performance will be ~10% worse inside of the VM.

7. **Reboot the host**
