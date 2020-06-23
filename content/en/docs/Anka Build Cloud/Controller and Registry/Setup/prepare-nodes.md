---
date: 2019-07-26
title: "Preparing your Anka Nodes"
linkTitle: "Preparing your Anka Nodes"
weight: 3
description: >
  Prepare high availability macOS machines before connecting to the Anka Build Cloud Controller
---

Once the Anka Build Virtualization software has been installed onto a macOS machine, you'll typically want to ensure that the machine has high availability. This requires turning off sleep and several other default features that would cause the machine to become unavailable. Below are the preparatory steps we suggest:

1. **Enable automatic login for the current user:** Go to Preferences > Users > enable automatic login.

2. **Disable require password after screensaver has started:** Go to Preferences > Security > under General > uncheck `require password after screensave or sleep begins` option.

3. **Disable all forms of sleep:** Go to Preferences > Energy Saver > disable any sleep options.

    Alternatively, you can do this from the command-line:

    ```shell
    # Never go into computer sleep mode
    systemsetup -setcomputersleep Off
    # Disable standby
    pmset -a standby 0
    # Disable disk sleep
    sudo pmset -a disksleep 0
    # Hibernate mode is a problem on some mac minis; best to just disable
    Sudo pmset -a hibernatemode 0 
    ```

4. **Disable spotlight:** `sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist`

    If you cannot perform launchctl commands, you can execute these commands from the command-line:

    ```shell
    # Disable indexing volumes
    defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes"
    defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network"
    killall mds
    # Make sure indexing is DISABLED for the main volume
    mdutil -a -i off /
    mdutil -a -i off
    ```

5. **Reboot the host**
