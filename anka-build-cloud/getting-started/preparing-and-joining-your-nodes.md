---
date: 2019-12-12T00:00:00-00:00
title: "Preparing and Joining your Nodes to the Controller"
weight: 4
description: >
  How to join your Anka Build Virtualization Nodes to the Anka Build Cloud Controller
---

## Preparing your Nodes

Once you've set up your Controller & Registry, you'll want to install Anka Build Virtualization software onto a macOS machine and join it to your Build Cloud.

{{< hint warning >}}
This guide has a few options that are typically not available on MDM controlled machines. You may need to ask your system administrators/local IT team to allow them.
{{< /hint >}}

{{< hint warning >}}
Be sure to reboot the host after applying these changes.
{{< /hint >}}

### Host Preparation (Optional, but Recommended)

- **Enable `Automatic Login` for the current user:** Go to Preferences > Users > Enable Automatic Login for the current user. Or, use `sudo sysadminctl -autologin set -userName {USERTOENABLE} -password {PASSWORD}`.

- Disable spotlight on the host and also inside of the VM:

    If you cannot perform launchctl commands, you can execute these commands from the command-line:

    ```shell
    sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes"
    sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network"
    sudo killall mds || true
    sleep 60
    sudo mdutil -a -i off /
    sudo mdutil -a -i off
    sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist
    sudo rm -rf /.Spotlight-V100/*
    ```

    MDS can be disable entirely with `sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist`, but only if SIP is disabled on the host (not recommended).

{{< hint info >}}
You may also want to have your nodes restart on host level failure: `systemsetup -setrestartpowerfailure on` & `systemsetup -setrestartfreeze on`
{{< /hint >}}

## Joining Prerequisites

- [Anka Build Cloud Controller should be configured and running before your Anka Node can join.]({{< relref "anka-build-cloud/_index.md" >}})

- [Your Node should be licensed.]({{< relref "licensing.md" >}})

- [Your Node should be prepared for high availability.]({{< relref "#preparing-your-nodes" >}})

## Add the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

## Push the VM to the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

## Join to the Controller & Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_join-to-cluster.md" >}}

## Start a VM instance using the Controller UI

{{< include file="_partials/anka-virtualization-cli/getting-started/_start-vm-instance-using-controller-ui.md" >}}

<!-- ### Joining as a non-sudo user

It is also possible to join without needing `sudo`. However, as of right now, this has a problems you need to consider:

1. When you upgrade the Anka Build Controller software, there is an automatic agent update triggered for each one of your Anka Nodes. This agent communicates with the Controller to pick up tasks from the queue. Since Apple's `installer` command requires root, this process will not work when running the agent as a non-sudo user. The solution is to disjoin nodes before upgrading your Build Cloud and then issue `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt / ` (`AnkaAgentArm.pkg` if using Anka 3.0) on the nodes after it's running.

To join with a non-sudo user, you simply run `ankacluster join http://anka.controller --force-no-sudo`. -->

## Disjoining

{{< hint info >}}
You don't need to disjoin nodes to upgrade the Anka Virtualization package.
{{< /hint >}}

```shell
❯ sudo ankacluster disjoin
Disjoined from the Anka Cloud Cluster
```

## Answers to Frequently Asked Questions

- Errors like the following are typically the cause of an older version of the agent on your node. You'll want to visit http://downloads.veertu.com, download the agent version matching your controller's version, and then try your ankacluster join command again:
    ```bash
    > sudo ankacluster join http://anka.controller --reserve-space 20GB
    Testing connection to controller...: Ok
    Testing connection to the registry...: Ok
    Ok
    exit status 2
    Log file created at: 2021/07/16 12:36:10
    Running on machine: hostname1
    Binary: Built with gc go1.14.3 for darwin/amd64
    Log line format: [IWEF]mmdd hh:mm:ss.uuuuuu threadid file:line] msg
    I0716 12:36:10.164826  8172 log_cleaner.go:34] clean /var/log/veertu/anka_agent*...
    I0716 12:36:10.166385  8172 log_cleaner.go:56] don't touch: /var/log/veertu/anka_agent.hostname1.root.log.INFO.20210716-123610.8172
    I0716 12:36:10.172275  8172 main.go:173] starting ankaAgent
    I0716 12:36:10.172282  8172 main.go:174] Anka Agent version: 1.7.1-9545c9f5
    I0716 12:36:10.965625  8172 runner.go:43] Anka version is 2.4.1
    I0716 12:36:10.965806  8172 runner.go:46] Agent Version is v2
    Joining cluster failed
    ```
- The Controller will be checking disk space on every pull/preparation of a VM. If not enough disk space is available, it will automatically delete VM Templates/tags from the Node based on **which has the oldest last used timestamp** until there is enough space for the VM Template/Tag. This cannot be disabled at the moment. This can be modified to some extent by using --reserve-space flag (see explanation above)
- The registry `External URL` (seen in the controller UI/config) is used by the Nodes to pull down templates and tags. Be sure to set this URL properly in your Build Cloud Configuration and ensure firewalls allow communication.
