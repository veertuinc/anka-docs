---
title: "VM creation is stuck or failing"
linkTitle: "VM creation is stuck or failing"
weight: 1
---

## Scenarios

- The `anka create` runs for a while but gets stuck. Typically stuck at `prebooting` or `waiting for postinstall complete`.
- The Anka app creation process runs for hours and never completes.
- `installation failed: Bad address`
- `AMRestorePerformRestoreModeRestoreWithError failed with error: 1`
- `status 70` or `installation failed`, especially after upgrading the host to 15.5 or above.

{{< hint info >}}
To get more verbose errors, run `ANKA_CLICK_DEBUG=1 anka --debug create...`. If the click scripts fail, you can get the `/tmp/_last.png` generated when `ANKA_CLICK_DEBUG=1` to see what the last screen was in the VM before the failure.
{{< /hint >}}


## Common Causes and solutions

1. `status 70` can mean several things. It's a generic error from Virtualization APIs on macOS. The main ways to fix it are:
    - Uninstall Anka CLI fully (`sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`), then reinstall it.
    - Install Device Support or the latest Xcode on the host: https://downloads.veertu.com/anka/DeviceSupport-15.4.pkg
    - If attempting to set/use `http_proxy` or `https_proxy`, they will not work. There is a requirement for full internet access to set up macOS properly in later 15.x VM versions. You'll need to use `ANKA_NETWORK_MODE=disconnected` when creating the VM to temporarily disable networking entirely and eliminate the requirement.
    - You're trying to anka create a VM through SSH with `sudo anka create...`. Apple's security changes in later macOS versions make this problematic. You should use VNC > Terminal to create the VM instead as sudo, or, just create it as the current non-root user.
    - The required apple URLs are not whitelisted in your firewall/proxy. See https://support.apple.com/101555 for more information.
    - If the VM log shows `The virtual machine failed to start`, this is a problem with the Virtualization APIs accessing the keychain. Follow these steps:
        1. Log into VNC.
        2. Go under System Preferences > Apple Intelligence & Siri.
        3. Enable Siri, wait 5 minutes, and then disable it (disable if it's enabled already). Wait another minute or two for the Intelligence stuff to turn off fully.
        4. Then, delete all files under ~/Library/Keychains .
        5. Now go to System Preferences > Users & Groups > clicked on the (i) icon for the main user, and then do Change for Password.
        6. Reboot the host.
        7. Then try to open open /System/Library/CoreServices/Applications/Keychain\ Access.app and make sure you can log in and see the login keychain show in the list.
1. `AMRestorePerformRestoreModeRestoreWithError failed with error: 1` means that the host and VM have no internet connection.
1. `installation failed: Bad address` is a generiic error and you need to run the creation again with `anka --debug create...`.
1. Anka Creation, especially for 15.x and above, requires networking to be enabled.
1. Anti-virus and firewalls (even the apple firewall) are blocking networking initialization. They need to be disabled, or anka processes whitelisted.
2. The macOS installation has failed for some reason. You can open the `anka view {name}` to ensure that it is actually stuck or failed.
    - If so, try running `anka --debug create...` from your terminal to see if it works a second time. If it keeps failing, report the issue to Veertu support with the output from the `anka --debug create` command.
    - It's also possible that you haven't given enough resources to the VM being created. We recommend a minimum of 2 vcpus and 4GBs of memory for stability.


## Examples of errors

On creation (status 70):

```bash
Fri Sep 26 09:44:34 10 (create) 59386: installing macOS 26.0.0...

ankahv-arm64: /Users/nathanpierce/Library/Application Support/Veertu/Anka/img_lib/UniversalMac_26.0_25A354_Restore.ipsw: failed to install macOS
Fri Sep 26 09:45:11 40 (create) 59386: 652a213b-b1a6-496c-ac9e-6b6a2c72cda4: failed to install /Users/nathanpierce/Library/Application Support/Veertu/Anka/img_lib/UniversalMac_26.0_25A354_Restore.ipsw: status 70
Fri Sep 26 09:45:11 40 (create) 59386: 652a213b-b1a6-496c-ac9e-6b6a2c72cda4: installation failed
anka: installation failed
```

In the ~/Library/Logs/Anka/anka.log, you will see the following errors:

1. When `gs.apple.com` is not whitelisted

    ```bash
    Fri Sep 26 09:35:39 40 (install) 58032: (null): installation failed: Error Domain=VZErrorDomain Code=10007 "Installation failed." UserInfo={NSLocalizedFailure=An error occurred during installation., NSLocalizedFailureReason=Installation failed., NSUnderlyingError=0x60000135c900 {Error Domain=com.apple.MobileDevice.MobileRestore Code=3004 "Failed to copy auth install options in DFU mode." UserInfo={NSLocalizedDescription=Failed to copy auth install options in DFU mode., NSLocalizedFailureReason=An unknown error occurred during installation.}}}
    Fri Sep 26 09:35:39 40 (install) 58032: (null): virtual machine stopped with error: Error Domain=VZErrorDomain Code=4 "Transition from state "stopped" to state "stopping" is invalid." UserInfo={NSLocalizedFailure=Invalid virtual machine state transition., NSLocalizedFailureReason=Transition from state "stopped" to state "stopping" is invalid.}
    Fri Sep 26 09:35:39 40 (install) 58032: failed to install macOS: Error Domain=VZErrorDomain Code=10007 "Installation failed." UserInfo={NSLocalizedFailure=An error occurred during installation., NSLocalizedFailureReason=Installation failed., NSUnderlyingError=0x60000135c900 {Error Domain=com.apple.MobileDevice.MobileRestore Code=3004 "Failed to copy auth install options in DFU mode." UserInfo={NSLocalizedDescription=Failed to copy auth install options in DFU mode., NSLocalizedFailureReason=An unknown error occurred during installation.}}}
    ```

2. When `wkms-public.apple.com` is not whitelisted:

    ```bash
    Fri Sep 26 09:43:24 40 (install) 59152: (null): installation failed: Error Domain=VZErrorDomain Code=10007 "Installation failed." UserInfo={NSLocalizedFailure=An error occurred during installation., NSLocalizedFailureReason=Installation failed., NSUnderlyingError=0x600002c34d20 {Error Domain=com.apple.MobileDevice.MobileRestore Code=-1 "AMRestorePerformRestoreModeRestoreWithError failed with error: 1109" UserInfo={NSLocalizedDescription=AMRestorePerformRestoreModeRestoreWithError failed with error: 1109, NSLocalizedFailureReason=An unknown error occurred during installation.}}}
    Fri Sep 26 09:43:24 40 (install) 59152: (null): virtual machine stopped with error: Error Domain=VZErrorDomain Code=4 "Transition from state "stopped" to state "stopping" is invalid." UserInfo={NSLocalizedFailure=Invalid virtual machine state transition., NSLocalizedFailureReason=Transition from state "stopped" to state "stopping" is invalid.}
    Fri Sep 26 09:43:24 40 (install) 59152: failed to install macOS: Error Domain=VZErrorDomain Code=10007 "Installation failed." UserInfo={NSLocalizedFailure=An error occurred during installation., NSLocalizedFailureReason=Installation failed., NSUnderlyingError=0x600002c34d20 {Error Domain=com.apple.MobileDevice.MobileRestore Code=-1 "AMRestorePerformRestoreModeRestoreWithError failed with error: 1109" UserInfo={NSLocalizedDescription=AMRestorePerformRestoreModeRestoreWithError failed with error: 1109, NSLocalizedFailureReason=An unknown error occurred during installation.}}}
    ```

3. And finally, blocking `fcs-keys-pub-prod.cdn-apple.com` causes

    ```bash
    Fri Sep 26 09:45:11 40 (install) 59390: (null): installation failed: Error Domain=VZErrorDomain Code=10007 "Installation failed." UserInfo={NSLocalizedFailure=An error occurred during installation., NSLocalizedFailureReason=Installation failed., NSUnderlyingError=0x600000eb48a0 {Error Domain=com.apple.MobileDevice.MobileRestore Code=-1 "AMRestorePerformRestoreModeRestoreWithError failed with error: 1109" UserInfo={NSLocalizedDescription=AMRestorePerformRestoreModeRestoreWithError failed with error: 1109, NSLocalizedFailureReason=An unknown error occurred during installation.}}}
    Fri Sep 26 09:45:11 40 (install) 59390: (null): virtual machine stopped with error: Error Domain=VZErrorDomain Code=4 "Transition from state "stopped" to state "stopping" is invalid." UserInfo={NSLocalizedFailure=Invalid virtual machine state transition., NSLocalizedFailureReason=Transition from state "stopped" to state "stopping" is invalid.}
    Fri Sep 26 09:45:11 40 (install) 59390: failed to install macOS: Error Domain=VZErrorDomain Code=10007 "Installation failed." UserInfo={NSLocalizedFailure=An error occurred during installation., NSLocalizedFailureReason=Installation failed., NSUnderlyingError=0x600000eb48a0 {Error Domain=com.apple.MobileDevice.MobileRestore Code=-1 "AMRestorePerformRestoreModeRestoreWithError failed with error: 1109" UserInfo={NSLocalizedDescription=AMRestorePerformRestoreModeRestoreWithError failed with error: 1109, NSLocalizedFailureReason=An unknown error occurred during installation.}}}
    ```

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
