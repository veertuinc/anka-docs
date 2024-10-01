---
date: 2018-03-06
title: "Release Notes (CLI)"
linkTitle: "Release Notes"
weight: 100
---

{{< hint warning >}}
Not all plugins are maintained by Veertu Inc developers. You might not see them listed here.
{{< /hint >}}

## Current Version

### 3.5.1 (3.5.0.192) - October 1st, 2024

{{< hint info >}}
Addons upgrade is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- 15.0 Host OS currently has an issue where connecting to the VM over port forwarding for the first time after installing Anka will show a prompt on the host's desktop saying `"Allow ankahv-arm64 to find devices on local networks"`. You will need to manually approve this until a solution is provided by Apple.
- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- [Download Anka-3.5.1.192.pkg](https://downloads.veertu.com/anka/Anka-3.5.1.192.pkg)
- **Improvement:** Anka Develop now supports Macbook Air.
- **Bug Fix:** SIP disable was broken when creating VMs on EC2. `failed to disable SIP: Operation timed out`
- **Bug Fix:** For `anka create`, autologin enabling was not working on intel.
- **Bug Fix:** Fixed `anka create` support for 10.X macOS and the inability to get networking. Note: Networking on 10.X VMs requires building and installing https://github.com/pmj/virtio-net-osx kext.

## Previous Versions

### 3.5.0 (3.5.0.191) - September 19th, 2024

{{< hint info >}}
Addons upgrade is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- 15.0 Host OS currently has an issue where connecting to the VM over port forwarding for the first time after installing Anka will show a prompt on the host's desktop saying `"Allow ankahv-arm64 to find devices on local networks"`. You will need to manually approve this until a solution is provided by Apple.
- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- [Download Anka-3.5.0.191.pkg](https://downloads.veertu.com/anka/Anka-3.5.0.191.pkg)
- **Improvement / Bug Fix:** Major networking improvements for Intel VMs. You'll see network performance match that of the host inside of VMs, as well as more stability for large transfers.
- **Improvement:** On ARM, you can now suspend VMs from the Anka App/UI. Note: Suspending from the CLI/Terminal will still stop the VM when issuing the suspend command due to an inability to move a suspended VM from one host to another (Apple limitation).
- **Bug Fix:** Anka click inside of VM cannot use simple text like ("Allow") to click on buttons.
- **Bug Fix:** Full 15.0 GA `anka create` automation support for intel and arm (note: previous versions >= 3.3.9 already supported running 15.0 VMs).
- **Bug Fix:** Immediately suspending VMs after start on arm would cause a failure.


### 3.4.2 (3.4.2.190) - August 21th, 2024

{{< hint info >}}
Addons upgrade is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- [Download Anka-3.4.2.190.pkg](https://downloads.veertu.com/anka/Anka-3.4.2.190.pkg)
- **Improvement:** When using shared networking with a fixed MAC address, VMs can now use the same MAC address inside and we'll handle obtaining separate IPs dynamically. This'll allow licensing software that's fixed to a specific MAC to function for builds and tests.
- **Bug Fix:** 15.0 beta 5 SIP setup failure when running `anka create`.
- **Bug Fix:** 14.6.1 VNC was not being enabled properly.


### 3.4.1 (3.4.1.189) - August 12th, 2024

{{< hint info >}}
Addons upgrade is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- [Download Anka-3.4.1.189.pkg](https://downloads.veertu.com/anka/Anka-3.4.1.189.pkg)
- **Improvement:** Support for 15.0 betas with anka create.
- **Bug Fix:** Simulators attempting to access the VM's microphone would receive a prompt users and break testing. Audio device usage in VM no longer causes the prompts.

### 3.4.0 (3.4.0.188) - July 15th, 2024

{{< hint info >}}
Addons upgrade is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **New Feature:** iCloud login now works with VM + Host running 15.0 beta2 (or above).
- **New Feature:** Anka click scripts now support text targeting/clicking with a simple pattern. For example, if I wanted to click a button with the text OK in it, I'd use: `("OK")`. Read more (here)[https://github.com/veertuinc/anka-click-scripts].
- **New Feature:** Offline licensing is now possible for customers without an internet connection. Contact support for more information.
- **New Feature:** The System Proxy settings are now used, if available, for `anka create` and `anka license activate`.
- **New Feature:** In situations where the registry is down, but your desired Template/Tag is cached on Nodes, you can change `anka config pull_failback` to `1` and it will skip pulling if it already has the Template/Tag. This will only work for Anka CLI users, and not through the Build Cloud Controller & Registry.
- **New Feature:** We are now including a real "Apple Virtual Sound Device" inside of the VM for users who cannot use the Null Audio Device.
- **New Feature:** You can now stream import VMs using the CLI (`curl uuid.tar | anka import`).
- **Improvement:** Checksumming for `anka pull` -- This feature will prevent `anka pull` from downloading VMs that are corrupt or incomplete.
- **Improvement:** RFB VNC (password-less) is now allowed again for VMs (intel only). To enable, you must first change `[sudo] anka config vnc_nopassword 1` and then ensure the VM Template also has it enabled `anka modify VM display --vnc on`.
- **Bug Fix:** `anka create` failed with 14.5 and 15.x betas.
- **Bug Fix:** `anka registry --insecure add` was not working.
- **Bug Fix:** `10.15.7` setup scripts were failing for `anka create`.
- **Bug Fix:** ARM VMs now show suspended if they are suspended in the Anka.app GUI.
- **Bug Fix:** Suspending a VM can now function even if an IP wasn't assigned.
- **Bug Fix:** Post start, the MAC would lose trailing zeroes.
- **Bug Fix:** A double quote in VNC password breaks anka show (intel only).
- **Bug Fix:** Trying to set experimental nat networking mode was not possible from CLI (`nat: bad network mode`).
- **Bug Fix:** Two VMs get same MAC and IP assigned (intel only).



### 3.3.10 (3.3.10.185) - Apr 17th 2024

{{< hint info >}}
Addons upgrade is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **New Feature:** `ANKA_DEFAULT_USER_NAME` can be set to change the name used when creating the VM's user.
- **Improvement:** Better error indicating `chunk_size` is not currently supported for `anka export`.
- **Improvement:** MacOS automated setup scripts now work when Siri screen is enabled.
- **Bug Fix:** `anka export` bug would cause `import` to fail with `could not find the VM configuration`.
- **Bug Fix:** Randomly bridge mode users would get the wrong interface attached to the VM (en1 instead of en0).
- **Bug Fix:** Tag names with `+`, like `anka registry revert -t "brew+git+dotnet+commonsetup"`, would throw `anka: 400 Did not find any versions with tag brew git dotnet commonsetup`.

### 3.3.9 (3.3.9.182) - Feb 27th 2024

{{< hint info >}}
Addons upgrade is required for VM Swap feature, but not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- Nested virtualization is not functional inside of VMs yet.
- The `anka view` command is partially broken and requires double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **New Feature:** [(ARM ONLY) Ability to set VM swap location to a specific host level location.]({{< relref "whats-new/anka-3.3.9/index.md#ability-to-set-vm-swap-location-to-a-specific-host-level-location" >}})
- **New Feature:** [VM Export and Import v2: Tag support.]({{< relref "whats-new/anka-3.3.9/index.md#export-and-import-v2" >}})
- **Improvement:** Adde `anka config no_local` for setting for all VMs on host.
- **Improvement:** Better error message for licensing/clock drift.
- **Improvement:** Network performance improvements.
- **Bug Fix:** Heavy network activity inside of the VM would cause it to lose network connectivity and never recover.
- **Bug Fix:** Interrupting `anka pull` would orphan a partially downloaded VM and block subsequent pulls.
- **Bug Fix:** VM conversion process would rarely create unbootable VM.
- **Bug Fix:** After setting resolution to exact size of host, trying to full screen would crash Anka.
- **Bug Fix:** Creating VMs from GUI was crashing.
- **Bug Fix:** `ankanetd` was starting post-host-boot andpersisting even when no VMs were running.
- **Bug Fix:** Modifying disk then resizing using disktuil was causing VM to not boot.


### 3.3.8 (3.3.8.178) - Jan 2nd 2024

{{< hint info >}}
Addons upgrading is not required.
{{< /hint >}}

{{< hint warning >}}

ARM/Silicon specific issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **New Feature:** [Two new CLI options: `anka import` and `anka export` allows you to create an archive for a specific VM so it can be easily moved between hosts.]({{< relref "whats-new/anka-3.3.8/index.md" >}})
- **Improvement:** Support for Apple Silicon M3.
- **Improvement:** Support for more vmnet.framework interfaces in large setups.
- **Improvement:** `.{UUID}.plain.ank` files are now included in the garbage cleanup, optimizing disk space.
- **Improvement:** `Bad address` errors should now show a more accurate error message as to what is wrong.
- **Improvement:** `anka registry --api-key` now supports relative paths.
- **Bug Fix:** Automation for `anka create` can sometimes fail to enable autologin.
- **Bug Fix:** `bytes` field in `check-download-size` response was miscalculated.

### 3.3.7 (3.3.7.173) - Oct 13th 2023

- **Bug Fix:** Anka Create failures for Sonoma OS VMs only on AWS EC2 MAC Instances: `create: failed to execute script, status 74`/`click: error: Operation not permitted`/`click: failed to receive screenshot: Operation not permitted`.

### 3.3.6 (3.3.6.172) - Oct 10th 2023

{{< hint info >}}
- Addons upgrading is not required.
{{< /hint >}}

{{< hint warning >}}

ARM issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** Support for Sonoma Host OS.
- **Improvement:** Stability / Support for Sonoma VM creation.
- **Improvement:** Pulls from registry will now handle downloads that get interrupted better to avoid `failed to checkout: Input/output error` and other corrupt .ank file problems.
- **Bug Fix:** Preventing rare `maximum number of running instances exceeded` errors when starting VMs.
- **Bug Fix:** Fix for `process early exit status 6`/`failed to connect: No such file or directory`.
- **Bug Fix:** `hypervisor failed with status 256`/`failed to start: No child processes`/`ankanetd` hanging and blocking VM starts.
- **Bug Fix:** `anka delete` would rarely hang.
- **Bug Fix:** `anka clone -c` was not creating a fully unique copy (intel).
- **Bug Fix:** Both `--machine-readable` and `--debug` can be used at the same time. Note: JSON will go to STDOUT while debug to STDERR.
- **Bug Fix:** VMs which lose networking for whatever reason will now change to the proper failed state.
- **Bug Fix:** VM networking would suddenly fail (`ankanetd` would crash) under high CPU conditions with `ankanet: 22: network connection closed`
- **Bug Fix:** VM image chains were growing too large. This should prevent max file descriptor issues. We've also added an increase to the `maxfiles` for the hosts running Anka which should help existing customers with larger chains avoid failures.
- **Bug Fix:** Fixed problems with 10.13 and 10.14 networking running 2.5.6 addons.


### 3.3.5  (retracted; production impacting bugs)

### 3.3.4 (3.3.4.169) - July 26th 2023

{{< hint info >}}
- Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

ARM issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Docker virtualization processes were being counted when determining amount of running VMs.
- **Bug Fix:** Sudden VM networking failures under heavy load (`error 55` and `port_fwd: 25: socket error: Operation timed out`)
- **Bug Fix:** Various fixes for 13.5 and Sonoma VM creation failure.

### 3.3.3 (3.3.3.168) - July 10th 2023

{{< hint info >}}
- Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

ARM issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** 2023 M2 hardware support.
- **Bug Fix:** Disabling SIP in Sonoma was failing.
- **Bug Fix:** EC2 Users: `anka show` was hitting a deadlock on start.
- **Bug Fix:** Anka create fails if the VM can't access the internet or the host is using a proxy.


### 3.3.2 (3.3.2.166) - June 23th 2023

{{< hint info >}}
- Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

ARM issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** `anka create` now uses [anka click scripts](https://github.com/veertuinc/anka-click-scripts/tree/main) for enabling auto-login.
- **Bug Fix:** `anka create` would randomly show `failed to enable VNC: Bad address`.
- **Bug Fix:** `anka show` was hitting a deadlock.
- **Bug Fix:** Some older addons were not able to be read.


### 3.3.1 (3.3.1.165) - June 13th 2023

{{< hint info >}}
- Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

ARM issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** Support for macOS Sonoma.
- **Bug Fix:** Support for older Anka 2.x addons.

### 3.3.0 (3.3.0.164) - May 31st 2023

{{< hint info >}}
- Addons upgrading is not required but is recommended.
- (ARM ONLY) Delete and re-pull VM templates to host machines to take advantage of new performance and smaller disk usage changes.
{{< /hint >}}

{{< hint warning >}}

ARM issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` is partially broken and require double clicking the VM name in the Anka.app VM listing. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **New Feature:** Apply [VM Networking IP Filtering]({{< relref "whats-new/anka-3.3.0/index.md#vm-networking-ip-filtering" >}}) rules for VMs.
- **New Feature:** [Automated log in for autologin disabled VMs]({{< relref "whats-new/anka-3.3.0/index.md#automated-log-in-for-autologin-disabled-vms" >}}).
- **New Feature:** [(ARM ONLY) Support for FileVault in VMs]({{< relref "whats-new/anka-3.3.0/index.md#support-for-filevault-arm" >}}).
- **New Feature:** [(ARM ONLY) Trigger click scripts from within VMs, allowing UI specific interactions.]({{< relref "whats-new/anka-3.3.0/index.md#anka-click-scripts-inside-vm-arm" >}})
- **Improvement:** ARM and Intel PKGs have now been combined into a single installer.
- **Improvement:** `anka delete --cache` will also clean `vm_lib` directories.
- **Improvement:** To increase registry IO efficiency, we are now reusing TCP connections during registry operations.
- **Improvement:** Arm users will notice `anka config default_format` is now set to `1`. This indicates that the new 3.3.0 image format will be used, increasing VM performance and decreasing disk usage by VM templates significantly.
- **Bug Fix:** `anka start -v` was not opening the Viewer window.
- **Bug Fix:** Fix for `failed to enable VNC: Operation timed out` while running `anka create`.
- **Bug Fix:** Fixes for `failed to get status of task after start: Operation timed out`, `ankanet: failed to get response, error 2`, and `failed to start: No such file or directory`.
- **Bug Fix:** Registry show output shows a failed status.
- **Bug Fix:** Eliminated `failed to open config (config.yaml): No such file or directory` log spam.
- **Bug Fix:** Creating VM on EC2 M1 was throwing `setup failed: Connection refused`.
- **Bug Fix:** Nested Virtualization is now disabled when `hw.hvapic` is enabled.
- **Bug Fix:** `anka stop -f` and `anka start` would rarely become stuck.

### 3.2.1 (3.2.1.155) (Intel) - Feb 15th 2023

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

- **Bug Fix:** Marketplace AMI licensing fixes.

### 3.2.2 (3.2.2.158) (ARM) - March 13th 2023

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** License fixes/support for M1 and M2 hardware.

### 3.2.1 (3.2.1.157) (ARM) - Feb 15th 2023

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Marketplace AMI licensing fixes.

### 3.2.0 (3.2.0.154) (Intel) - Jan 9th 2023

{{< hint info >}}
Addons upgrading from Anka 2.x is not required but is recommended.
{{< /hint >}}

- VMs suspended on Anka 2.x will need to be re-suspended on 3.x.
- The Controller Agent version that comes with Anka will start at `1.30.1` (ARM will be `1.22.0`). **Older versions of the Controller do not support Anka 3. You will need to upgrade your controller to at least 1.30.1.**
- Previously, `anka create` would create a suspended VM. Starting in this version, VMs are stopped.
- GUI (Anka.app) VM creation will produce a VM without macOS set up.
- `anka --machine-readable registry list` has a key name change from `id` to `uuid`.
- The `modify set` commands are deprecated and will not show in the `anka modify --help` output. Please migrate to using the new modify commands instead.
- Only macOS 10.15 guest version and above are supported by Veertu. If you're using an older version, do not upgrade addons to 3.x and they will continue to function.

### 3.2.0 (3.2.0.153) (ARM) - Nov 16th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accessing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** Network stability (jenkins agents would sometimes suddenly disconnect).
- **Improvement:** Stability for Anka Create macOS setup process (VNC timeout, etc).
- **Improvement:** [Anka Click](https://github.com/veertuinc/anka-click-scripts) script support: Enabling "Reduce transparency" post-`anka create` to avoid dynamical wallpapers and other background changes from breaking image clicking.
- **Improvement:** Partially downloaded files (dot files under `img_lib_dir`) from `anka registry pull` are cleaned up after they are older than 1 day.
- **Bug Fix:** Pull/Conversion is happened on every VM Start request.
- **Bug Fix:** Controller Agent coming with Anka package was being installed and forcefully upgrading the version users already have.
- **Bug Fix:** Anka run was hanging when the VM received a `sudo reboot` (supports packer's expect disconnect feature).
- **Bug Fix:** `ANKA_DEFAULT_USER` and `ANKA_DEFAULT_PASSWD` did not allow users to change the default user and pass in the VM when using `anka create`.

### 3.1.1 (3.1.1.152) (ARM) - Oct 25th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accesing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Pulling was converting each and delaying VM start requests.
- **Bug Fix:** Various anka create automation script fixes.

### 3.1.0 (3.1.0.151) (ARM) - Oct 17th 2022

{{< hint info >}}
Addons upgrading is not required but is recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` and `anka start -v` are partially broken and require double clicking the VM name in the Anka.app VM listing. Also, the Anka viewer requires you first start the VM with `anka start -v`. Both of these issues make VNC, which is enabled by default, a better route for accesing your VM.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Improvement:** MacOS installation through the `anka create` command is now automated.
    - SIP is now disabled by default inside of the VM.
    - VNC is now enabled by default inside of the VM.
- **Improvement:** VMs can be started in Recovery Mode from the CLI with `ANKA_START_MODE=2 anka start 12.6`.
- **Improvement:** The `anka create` command will now produce `stopped` state VMs (suspending is not supported by Apple yet).
- **Improvement:** [Shortened `anka modify` and `port-forwarding` command.]({{< relref "whats-new/anka-3.1.0/index.md#shortened-anka-modify-and-port-forwarding-command" >}})
- **Improvement:** Better handling of VM kernel panics.
- **Improvement:** Improved network performance and security.
- **Improvement:** The `anka list` command will now show the last accessed date of the VM.
- **Improvement:** Pulling an intel VM Template from the Registry will now throw a failure.
- **Improvement:** Anka license commands will warn about EULA needing to be accepted.
- **New Feature:** [The `anka create --list` command will display macOS versions available to target with `anka create -a` (which downloads the appropriate .ipsw and uses it for creation).]({{< relref "whats-new/anka-3.1.0/index.md#targeting-of-specific-macos-versions-with-anka-create--a" >}})
- **New Feature:** [You can now resize the VM's disk.]({{< relref "whats-new/anka-3.1.0/index.md#ability-to-resize-the-vms-disk" >}})
- **New Feature:** [Support for the Anka Build Cloud's UAKs when interacting with the registry commands.]({{< relref "whats-new/anka-3.1.0/index.md#support-for-the-anka-build-clouds-uaks-when-interacting-with-the-registry-commands" >}})
- **New Feature:** `ANKA_LOG_LEVEL="debug"` is available as a replacement for `anka --debug`.
- **Bug Fix:** `anka push -r` was not working to specify registry url to push to.
- **Bug Fix:** Disabled beta program enrollment for macOS.


### 3.0.1 (3.0.1.144) (ARM) - May 12th 2022

{{< hint info >}}
Addons upgrading is not required for features in this release. However, it's always recommended.
{{< /hint >}}

{{< hint warning >}}

Known issues:

- Nested virtualization is not functional inside of VMs yet.
- `anka view` does not function post-vm-start unless you started the VM with -v.
- In Anka 2 we enabled a minimal VNC by default, however it is not available in Anka 3.
- iCloud/Apple logins will fail inside of the VM. You can still log into your account through Apple's website and download apps through your developer account. Or, transfer them from the host into the VM with `anka cp`.
- Changing the display resolution dynamically fails.
- Physical device capture outside of USB devices like keyboard and "pointing" is not possible.

{{< /hint >}}

- **Bug Fix:** Curling a forwarded port which does not have the inner VM service/port running will no longer crash the VM.
- **Bug Fix:** `anka --machine-readable show` now shows proper #G value under `ram` key's value, instead of bytes.
- **Bug Fix:** EULA acceptance from GUI was not sticking.
- **Bug Fix:** `anka --machine-readable describe` port forwarding rules had a key name of `name` instead of `rule_name` which was in Anka 2.
- **Bug Fix:** `anka --machine-readable describe` port forwarding rules had no `host_port` set which packer was looking for.
- **Bug Fix:** Screenshot features are now available in the CLI and GUI app.
- **Bug Fix:** Cloning two VMs after setting `anka modify` to `bridge` was showing a new `creation_date` and the same IP in use in anka show (but not in the VMs).
- **Bug Fix:** You can now use certs to authenticate with pull and push commands.
- **Bug Fix:** Anka GUI App no longer default to using 2vCPUs if more are available.
- **Bug Fix:** Deleting a tag will now delete the associated .ank files properly.
- **Improvement:** Trying to use .app installers will now throw a clear error that they are not supported.
- **Improvement:** Support for Mac Studio hardware.
- **Improvement:** Custom `vm_lib_dir` will not be automatically created so that it doesn't prevent network volumes from mounting to that location with the proper name on reboot of the host.
- **Improvement:** We now show the internal screen sharing connection string if there is a corresponding port forwarding rule.
- **Improvement:** Expired licenses or licenses that do not support the command you're running will now throw a clear error.
- **New Feature:** Bridge network isolation is now available.

### 3.0.0 (3.0.0.140) - Feb 1st 2022

- This is our first release! Features are detailed in the documentation throughout this site!
