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
- `status 70`, especially after upgrading the host to 15.5 or above.

{{< hint info >}}
To get more verbose errors, run `ANKA_CLICK_DEBUG=1 anka --debug create...`. If the click scripts fail, you can get the `/tmp/_last.png` generated when `ANKA_CLICK_DEBUG=1` to see what the last screen was in the VM before the failure.
{{< /hint >}}


## Common Causes and solutions

1. `status 70` can mean several things. It's a generic error from Virtualization APIs on macOS. The main ways to fix it are:
    - Uninstall Anka CLI fully (`sudo /Library/Application\ Support/Veertu/Anka/tools/uninstall.sh`), then reinstall it.
    - Install Device Support or the latest Xcode on the host: https://downloads.veertu.com/anka/DeviceSupport-15.4.pkg
    - If attempting to set/use `http_proxy` or `https_proxy`, they will not work. There is a requirement for full internet access to set up macOS properly in later 15.x VM versions. You'll need to use `ANKA_NETWORK_MODE=disconnected` when creating the VM to temporarily disable networking entirely and eliminate the requirement.
1. `AMRestorePerformRestoreModeRestoreWithError failed with error: 1` means that the host and VM have no internet connection.
1. `installation failed: Bad address` is a generiic error and you need to run the creation again with `anka --debug create...`.
1. Anka Creation, especially for 15.x and above, requires networking to be enabled.
1. Anti-virus and firewalls (even the apple firewall) are blocking networking initialization. They need to be disabled, or anka processes whitelisted.
2. The macOS installation has failed for some reason. You can open the `anka view {name}` to ensure that it is actually stuck or failed.
    - If so, try running `anka --debug create...` from your terminal to see if it works a second time. If it keeps failing, report the issue to Veertu support with the output from the `anka --debug create` command.
    - It's also possible that you haven't given enough resources to the VM being created. We recommend a minimum of 2 vcpus and 4GBs of memory for stability.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
