---
date: 2018-03-06
title: "Release Notes"
weight: 100
---

## Current Version

### 1.23.0 (1.23.0-3523c787) - Apr 19th, 2022

- **Bug Fix:** VM Capacity reverts after rebooting the node.
- **Bug Fix:** Retrying a registry push too fast will cause a failure.
- **Bug Fix:** Controller UI was reporting inaccurate sizes for disk usage.
- **Improvement:** Older CMD logs are now rotated and cleaned up once the aggregate size of logs reaches 700MB (unless otherwise specified).
- **Improvement:** Anka 3 (arm) Nodes are forced to a capacity of 2 (Apple's limitation) to prevent confusion.
- **New Feature:** [Request a VM with a specific VLAN & View available VLANs attached to a specific Host/Anka Node]({{< relref "Whats New/build-cloud-1.23.0/index.md" >}}) (REST API ONLY)
- (Standalone Registry: 1.23.0-f21df6a2)

---

## Previous Versions

### 1.22.0 (1.22.0-5dc750f1) - Jan 10th, 2022

- **Bug Fix:** Available Node and Template information will now show properly for failed instances.
- **New Feature:** [VM Templates, Instances, and Nodes will all show the architecture (intel or arm) they support.]({{< relref "Whats New/build-cloud-1.22.0/index.md" >}})
- (Standalone Registry: 1.22.0-5e54d0d6)

---

### 1.21.2 (1.21.2-39f47eee) - Dec 28th, 2021

{{< hint info >}}
This is a small patch release for 1.21.0.
{{< /hint >}}

- **Bug Fix:** ARM Agent was not being automatically installed correctly on upgrade (404 package download).
- **Bug Fix:** The etcd container could not start with certain docker-compose versions due to quotes in etcd.env.
- (Standalone Registry: 1.21.2-b3f4cfe7)

---

### 1.21.1 (1.21.1-5aebaf69) - Dec 15th, 2021

{{< hint info >}}
This is a small patch release for 1.21.0.
{{< /hint >}}

- **Bug Fix:** Within the docker package's controller/controller.env, the inline comment for `ANKA_ANKA_REGISTRY` would break URLs if the comment was not removed.
- (Standalone Registry: 1.21.1-77c8e66e)

---

### 1.21.0 (1.21.0-bcc26b24) - Dec 10th, 2021

- **Bug Fix:** After a macOS upgrade, the `/var/log/veertu` directory was being removed by Apple and not being recreated. We are not creating this if it doesn't exist so that logs are not missing when they're needed.
- **Bug Fix:** Fixed the automated agent upgrade processes that was causing the agent to get stuck on the host, post-controller upgrade.
- **Bug Fix:** Defragmentation of ETCD was happening more often than configured.
- **Improvement:** Centralized logs are now rotated, by default, each day. This will prevent logs from growing too large should rotation not be triggered.
- **Improvement:** Redesigned docker package. This includes changes to the tag we generate in dockerhub (it no longer supports certain flags/options in the Entrypoint). Most customers are already using ENVs, so they should be fine.
- _Minimum Registry version required for Controller - 1.19.0_
- (Standalone Registry: 1.21.0-ced10c21)

{{< hint warning >}}
##### Known issues

Please note that there is a temporary workaround required for a bug that started in versions after 1.18.0 of the Controller/Registry agent which runs on your nodes. All versions of the agent, when noticing that the version of itself does not match the version of the controller, will perform a self-upgrade and restart. The restart seems to be problematic on some setups and leaves a zombie anka_agent process and and Offline status in the controller UI. To work around the bug when upgrading your controller/registry, you'll need to change the existing steps to include:
- Disjoining all of the nodes first
- Do the controller/registry upgrade
- Run `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on each node to download the new version
- And finally join each node back

<br />

**1.21.0 fixes this issue, so it will not be necessary for future releases.** Thanks for your understanding and we are sorry for the inconvenience this causes.
{{< /hint >}}

---

### 1.20.0 (1.20.0-035872f5) - Nov 10th, 2021

- Bug Fix: Moving networks/IPs now updates the Node IP in the Controller UI/database.
- New Feature: [Delete VM Templates from Nodes through the Controller API.]({{< relref "Whats New/build-cloud-1.20.0/index.md#delete-vm-templates-on-node-from-controller-api" >}})
- New Feature: [Ability to see what VM Templates are on a node from the Controller API.]({{< relref "Whats New/build-cloud-1.20.0/index.md#view-availablepulled-vm-templates-on-a-node-from-controller-api" >}})
- Improvement: The `ankacluster` and Anka Agent now support M1/ARM.
- Improvement: Upgraded etcd to 3.5.1.
- Improvement: Various security patches & upgrades for golang.
- _Minimum Registry version required for Controller - 1.19.0_
- (Standalone Registry: 1.20.0-c83f487d)

> Known issues we're working on fixes for:
> Please note that there is a temporary workaround required for a bug that started in versions after 1.18.0 of the Controller/Registry agent which runs on your nodes. All versions of the agent, when noticing that the version of itself does not match the version of the controller, will perform a self-upgrade and restart. The restart seems to be problematic on some setups and leaves a zombie anka_agent process and and Offline status in the controller UI. To work around the bug when upgrading your controller/registry, you'll need to change the existing steps to include:
> - Disjoining all of the nodes first
> - Do the controller/registry upgrade
> - Run `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on each node to download the new version
> - And finally join each node back
> 
> We will be fixing the bug soon. Thanks for your understanding and we are sorry for the inconvenience this causes.

---

### 1.19.0 (1.19.0-7c1c1424) - Oct 4th, 2021
- Bug Fix: Registry files (tag files and some .ank) were rarely zeroing out due to bad read/write logic
- Improvement: Controller logs now show with node name instances start on
- (Standalone Registry: 1.19.0-76bca3a)

> Known issues we're working on fixes for:
> Please note that there is a temporary workaround required for a bug that started in versions after 1.18.0 of the Controller/Registry agent which runs on your nodes. All versions of the agent, when noticing that the version of itself does not match the version of the controller, will perform a self-upgrade and restart. The restart seems to be problematic on some setups and leaves a zombie anka_agent process and and Offline status in the controller UI. To work around the bug when upgrading your controller/registry, you'll need to change the existing steps to include:
> - Disjoining all of the nodes first
> - Do the controller/registry upgrade
> - Run `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` on each node to download the new version
> - And finally join each node back
> 
> We will be fixing the bug soon. Thanks for your understanding and we are sorry for the inconvenience this causes.

---

### 1.18.0 (1.18.0-b3bb21bf) - Aug 23rd, 2021
- Bug Fix: Reserved tasks do not get released back to queue
- Bug Fix: Prevent controller crashing due to ETCD related panics and when active ETCD endpoint cannot be reached
- New Feature: Ability to use certs and username/password for controller -> etcd connections | [Documentation]({{< relref "intel/Whats New/_index.md#ability-to-use-certs-and-usernamepassword-for-etcd-connections" >}})
- Improvement: Upgrading etcd binaries to 3.4.16
- (Standalone Registry: 1.18.0-04fd94e)

---

### 1.17.1 (1.17.1-4aead62f) - July 14th, 2021
- Bug Fix: Chrome based browsers don't work with root token and SSO/OpenID/Keycloak
- (Standalone Registry: 1.17.1-0966fcd)

---

### 1.17.0 (1.17.0-fcb89b75) - June 29th, 2021
- Improvement: The Node UUID is now stored in the Anka Agent plist to avoid it changing between crashes or restarts (be sure to disjoin and join after upgrading)
- Improvement: `ankacluster join` commands will now throw a warning if you have not accepted the Anka EULA
- Improvement: The exact commands the agent is running will be output to the logs in the event of an error
- Bug Fix: Binary parameters are ignored if preceding by an unknown parameter
- Bug Fix: Groups are now removed from Nodes if ETCD content is reset/deleted
- Bug Fix: `ankacluster join --vm-stuck-delay` is now functional again
- (Standalone Registry: 1.17.0-eb513cc)

---

### 1.15.0 (1.15.0-c69e2600) - Apr 6th, 2021
- Bug Fix: Client-side load-balancing with three etcd and/or controller containers throws `etcdserver: mvcc: required revision is a future revision` and causes a random loss of VMs
- New Feature: `script_result` object is now returned from [Save Image Template API]({{< relref "Anka Build Cloud/working-with-controller-and-API.md#list-save-template-image-requests" >}})
- Bug Fix: The start vm queue code sometimes check tasks from the controller queue. It doesn't take the tasks but it can create load on the db
- (Standalone Registry: 1.15.0-a88b7bc)

---

### 1.14.0 (1.14.0-17620328) - Feb 18th, 2021
- New Feature: [A button will now show in the Controller UI allowing you to delete an Offline node (instead of having to issue an API call)]({{< relref "intel/Whats New/_index.md#delete-button-will-show-for-offline-nodes" >}})
- New Feature: [You can now set a specific range of MAC addresses that are assigned to VM instances]({{< relref "intel/Whats New/_index.md#customize-the-range-of-mac-addresses-the-controller-uses-for-creating-vms" >}})
- Improvement: The registry now supports ANKA_ environment variables, similar to the controller
- (Standalone Registry: 1.14.0-1e39461)

---

### 1.12.0 (1.12.0-65cba643) - Dec 1st, 2020
- Bug Fix: Uninstaller throws exit code of 1
- New Feature: Allow configuration of TLS/SSL Protocols and Ciphers for Controller & Registry
- (Standalone Registry: 1.12.0-1f2c3e1)

---

### 1.11.2 (1.11.2-ae2c9036) - Nov 10, 2020
- Bug Fix: Memory leak in controller
- (Standalone Registry: 1.11.2-886e687)

---

### 1.11.1 (1.11.1-1df83172) - Oct 13, 2020
- Bug Fix: Cache building / Save Image was causing the node to go offline and terminating any other VMs running on it.
- (Standalone Registry: 1.11.1-b8cecfd)

---

### 1.11.0 (1.11.0-59d63cca) - August 31, 2020
- New Feature: `anka-controller` executable from macOS native package now supports `--tail {lines}`
- Improvement: Removed all unnecessary files from docker .tar.gz archive
- Bug Fix: Pulling a template/tag now calculates the size of the tag properly (instead of only the latest tag for the template)
- Bug Fix: VNC url in Controller UI > Instances page is now a link again
- (Standalone Registry: 1.11.0-d43f91c)

---

### 1.10.1 - August 20, 2020
- New Feature: [Unresponsive VM monitoring with `ankacluster join --enable-vm-monitor`]({{< relref "intel/Whats New/_index.md#unresponsive-vm-monitor" >}})

---

### 1.10.0 - August 5, 2020
- Bug Fix: Central Logging was preventing nodes from joining
- Bug Fix: Certificates with a space in the Organization (O=) were not supported when trying to set permission groups.
- Bug Fix: Controller crashes if client tries to show permission group after deletion

---

### 1.9.1 - June 23, 2020
- Bug Fix: When a user sets a custom img_lib_dir location, the anka agent doesn't calculate disk usage for the new location

---

### 1.9.0 - June 10, 2020
- New Feature: Management of MAC Addresses to prevent rare collision
- New Feature: MAC Address can be specified when creating a VM using the API

---

### 1.8.0 - June 16, 2020
- New Feature: Controller REST API now supports modifying the vCPU and RAM for VMs in a stopped state
- New Feature: Controller REST API Start VM Instance object allows for custom key/value metadata
- New Feature: [Controller Instances page now allows users to add custom columns (from metadata key/value)]({{< relref "intel/Whats New/_index.md" >}})
- New Feature: Updated license terms
- Bug Fix: Agent process stays alive despite disjoin
- Bug Fix: Instance sorting in controller UI was not kept on update
- Bug Fix: Sorting in instances page in dashboard is reversed
- Bug Fix: Agent's HTTP client would sometime fail to parse controller's responses

---

### Change Log combined package (Mac) version 1.7.1 - Apr 14, 2020
- Bug Fix: Anka agent miscalculates free space to keep when pulling images on the node

### Change Log combined package (linux) version 1.7.1 - Apr 14, 2020
- Bug Fix: Anka agent miscalculates free space to keep when pulling images on the node

---

### 1.7.0 - Mar 23, 2020
- Bug Fix: ankacluster doesn't pass max vm count to agent
- Bug Fix: Node upgrade request is sometimes sent more than once
- Bug Fix: Distribute not showing progress
- Bug Fix: Controller moves from ‘enterprise’ to ‘basic’ after uninstall and upgrade anka from 2.2.1 to 2.2.2
- New Feature: Display node storage usage information in the controller dashboard
- New Feature: Show size of templates in the controller dashboard
- New Feature: Show a link to the jenkins job that the vm is running in the controller dashboard
- New Feature: Put prompt while deleting template from the controller dashboard
- New Feature: add reserve disk flag in ankacluster command
- New Feature: Add the option to recover from a crash while uploading files to registry

---

### 1.6.0 - Mar 08, 2020
- Bug Fix: Jenkins plugin leaves zombie VMs after upgrade to version 2.204
- Bug Fix: Anka registry pull hangs forever when run from agent if VM has a lot of image files
- New Feature: Handle vm start failures more robustly
- New Feature: Added a fail timeout for template distribution

---

### (Mac) - Version 1.5.4 - Jan 15, 2020#
- Bug Fix: Version 1.5.3 was overwriting /usr/local/bin/anka-controllerd file

---

### (Linux) - Version 1.5.4 - Jan 15, 2020#
Other: Version number change to 1.5.4 to make it match with the mac package
