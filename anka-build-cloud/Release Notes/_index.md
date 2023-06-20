---
date: 2018-03-06
title: "Release Notes (Cloud)"
linkTitle: "Release Notes"
weight: 100
---

## Current Version

### 1.35.0 (1.35.0-525badf3) - June 20th, 2023

- **New Feature:** The Controller Agent running on Nodes will now timeout when `anka start` commands take longer than 1m30s to complete. This can be controlled by setting `ankacluster join --cli-start-timeout {new time here}`.
- **Improvement:** Controller logs will now indicate when a Node has picked up the task to start a VM.
- (Standalone Registry: 1.35.0-16331641)

---

## Previous Versions

### 1.34.0 (1.34.0-4cda29d3) - June 8th, 2023

- **Improvement:** (AUTH ONLY) The Controller will now exclusively use the Root Token for communication and authentication with the Registry. This means that both the Registry and Controller **must** have AUTH enabled as well as the same Root Token in their configs. Several ENVs like `ANKA_API_KEY_`, `ANKA_CLIENT_`, and `ANKA_OIDC_` (registry only) are no longer available and necessary. They can be removed from your configuration, but be sure to mirror the AUTH configuration for Root Token and other ENVS from the Controller.
- **Bug Fix:** Azure registry `version upload failed` errors.
- **Bug Fix:** User API Keys button was missing from UI.
- **Bug Fix:** Azure backed registry not returning entire list of templates from API.
- **Bug Fix:** Azure Registry failing to clean failed pushes.
- (Standalone Registry: 1.33.0-20746877)

### 1.33.0 (1.33.0-5465bb03) - March 16th, 2023

- **Bug Fix:** Getting distribution status returned as null body with OK status.
- **Bug Fix:** Distribution was blocked for Unhealthy nodes.
- **Bug Fix:** Distribution (/api/v1/registry/vm/distribute) API response randomly showing `progress: 1` (100%) but `status: false` indefinitely.
- **Improvement:** [New Permissions Management Panel]({{< relref "whats-new/build-cloud-1.33.0/index.md#new-permissions-management-panel" >}})
- **Improvement:** [Code/Explicit Flow support for OIDC.]({{< relref "whats-new/build-cloud-1.33.0/index.md#codeexplicit-flow-oidc-support" >}})
  - ENV `ANKA_OIDC_CACHE_TTL` has been deprecated.
- **Improvement:** ETCD upgraded to 3.5.7.
- **New Feature:** [Kubernetes NGINX Ingress Controller Header Passthrough support.]({{< relref "whats-new/build-cloud-1.33.0/index.md#support-certificate-auth-with-for-nginx-ingress-in-kubernetes" >}})
- (Standalone Registry: 1.33.0-a2d41374)

### 1.32.0 (1.32.0-c375584c) - Feb 6th, 2023

- **New Feature:** [Drain Mode, allowing users to prevent any *new* VMs from starting on the node, but not terminate already running VMs and the jobs using them]({{< relref "whats-new/build-cloud-1.32.0/index.md#drain-mode" >}}).
- **New Feature:** [Certificate Revocation.]({{< relref "whats-new/build-cloud-1.32.0/index.md#certificate-revocation" >}})
- **Bug Fix:** `/api/v1/node?id=SOME_ID` would return a different object from `/api/v1/node` (without ID).
- (Standalone Registry: 1.32.0-81cf056c)


### 1.31.1 (1.31.1-ab1c787e) - Jan 12th, 2023

- **Bug Fix:** Registry threw a panic when using OIDC.
- (Standalone Registry: 1.31.1-f997f56d)

### 1.31.0 (1.31.0-93a0aa2c) - Dec 28th, 2022

- **Bug Fix:** Streaming logs (from Controller's Logs page) created high CPU usage on registry.
- **Bug Fix:** With OIDC enabled, lots of unnecessary logs.
- **Bug Fix:** Show UAK String button replaced Copy Key String which didn't work on some browsers.
- **Improvement:** Controller Agent performance improvement for Anka Nodes with lots of templates.
- **New Feature:** [Support calling group claims from userinfo endpoint with `ANKA_OIDC_USER_INFO` ENV.]({{< relref "whats-new/build-cloud-1.31.0/index.md#ability-to-call-group-claims-from-userinfo-endpoint-with-anka_oidc_user_info-env" >}}) We have also added `ANKA_OIDC_CACHE_TTL` which allows control for how long we cache the returned group list from the endpoint as calls to this endpoint can have a performance impact.
- **New Feature:** [Ability to set custom scopes for OIDC with `ANKA_OIDC_SCOPES` ENV.]({{< relref "whats-new/build-cloud-1.31.0/index.md#ability-to-set-custom-scopes-with-anka_oidc_scopes-env" >}})
- (Standalone Registry: 1.31.0-5c7ab105)

### 1.30.1 (1.30.1-299dcb37) - Dec 20th, 2022

- **Bug Fix:** Anka Cluster/Controller Agent did not support 3.2.0 on intel.
- (Standalone Registry: 1.30.1-3cca9a3a)


### 1.30.0 (1.30.0-66973267) - Dec 1st, 2022

{{< hint warning >}}
1.28.0 had a significant change you should be aware of to prevent problems. Please read over [the ETCD Compaction and Defragmentation documentation to understand what has changed]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md#etcd-compaction-and-defragmentation" >}}).
{{< /hint >}}

- **Bug Fix:** VNC urls from the Controller Instances page now show the password properly in Anka 3.
- **Bug Fix:** Incorrect CPU displayed in dashboard and calculation of default value for --vcpu-override on arm machines.
- **Bug Fix:** Panic on `ankacluster join` when registry URL in the controller is not configured.
- **Bug Fix:** Anka cloud agent calculates ram incorrectly when doing resource check.
- **Bug Fix:** Anka cloud agent was terminating pulls while Anka 3 Arm conversion was happening, post pull, and it was reaching a timeout.
- **Improvement:** Simplified `csr_active_config` by removing `forceOff`.
- **Improvement:** Moved `processVMUpdate` logs to log level 1 so the Controller logs are less noisy.
- **New Feature:** [Ability to set `--local` and `--no-local` when starting VMs through API.]({{< relref "whats-new/build-cloud-1.30.0/index.md#ability-to-set---local-and---no-local" >}})
- (Standalone Registry: 1.30.0-f0b6506a)

### 1.29.0 (1.29.0-49077f79) - Oct 3rd, 2022

{{< hint warning >}}
1.28.0 had a significant change you should be aware of to prevent problems. Please read over [the ETCD Compaction and Defragmentation documentation to understand what has changed]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md#etcd-compaction-and-defragmentation" >}}).
{{< /hint >}}

- **Bug Fix:** Mac package anka-controllerd improperly uses `ANKA_LISTEN_ADDRESS` instead of `ANKA_LISTEN_ADDR` ENV.
- **Bug Fix:** Agent tries to install PKG even if download from controller fails.
- **Bug Fix:** Bad string for custom hardware variable (HVAPIC) when using VM start API endpoint.
- **New Feature:** [Management UI for UAKs]({{< relref "whats-new/build-cloud-1.29.0/index.md" >}})
- (Standalone Registry: 1.29.0-8daa18e1)

### 1.28.0 (1.28.0-347c73ea) - Sep 6th, 2022

{{< hint warning >}}
This release has a significant change you should be aware of to prevent problems. Please read over [the ETCD Compaction and Defragmentation documentation to understand what has changed]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md#etcd-compaction-and-defragmentation" >}}).
{{< /hint >}}

- **New Feature:** Nodes with an expired Anka license now show "Inactive (Invalid License)" in the Nodes UI and do not accept start VM tasks.
- **New Feature:** Ability to modifying Registry vm list cache expiration/TTL.
- **Improvement:** Disabled ETCD defragmentation by default (see the note above) and changed compaction time to 30m.
- **Bug Fix:** Endless CPU usage increase when the license of a single node in the cluster is removed or expires.
- **Bug Fix:** `Setting license to` log spam when the only node on a controller is disjoined or license expires.
- **Bug Fix:** Various security related concerns.
- (Standalone Registry: 1.28.0-39c3ded1)

---

### 1.27.0 (1.27.0-76c64d00) - Aug 8th, 2022

- **New Feature:** Start VM Instance API now supports `video_controller`, `csr_active_config`, and `hvapic`.
- **Bug Fix:** Registry files were being corrupted when the server crashed mid-push.
- **Bug Fix:** $instance_id does not interpolate for name_template in start vm api.
- **Bug Fix:** Log rotation/cleaning fails when an unknown file  seen in the directory.
- (Standalone Registry: 1.27.0-bc73bfc8)

---

### 1.26.0 (1.26.0-7f63ad8a) - Jul 21st, 2022

- **Bug Fix:** Anka Usage calculations were considering duplicate paths.
- **Bug Fix:** Registry would not start if central log directory was missing (Azure only).
- **Improvement:** Upgraded etcd to 3.5.4 to solve [data corruption issue](https://github.com/etcd-io/etcd/tree/main/CHANGELOG#production-recommendation).
- (Standalone Registry: 1.26.0-757a6a71)

---

### 1.25.0 (1.25.0-b2a027a4) - Jun 23rd, 2022

- **Bug Fix:** Agent panics when anka CLI start VM returns an error.
- **Bug Fix:** Disjoining does not clear the node task queue and can cause heavy usage over time for the Controller.
- (Standalone Registry: 1.25.0-c6de18fa)

---

### 1.24.1 (1.24.1-41a53e49) - May 25th, 2022

- **Bug Fix:** Instances would sometimes have a blank tag.
- **Bug Fix:** The agent will now communicate using `--cacert` instead of `--root-cert` (which was removed in Anka 3.x).
- **Improvement:** The registry will now use a memory cache to speed up requests for templates/tags. Any API calls to `/registry/v2/vm` or `/registry/vm` benefit from this (the controller api also queries these endpoints when making requests for template or tag information).
- (Standalone Registry: 1.24.1-d252eb93)

---

### 1.24.0 (1.24.0-28ac9d95) - May 17th, 2022

- **New Feature:** [Start VM Instance API `startup_script` monitoring]({{< relref "whats-new/build-cloud-1.24.0/index.md#start-vm-instance-api-startup_script-monitoring" >}})
- **Bug Fix:** Hanging pulling process is not being terminated by agent.
- **Bug Fix:** If a previous upgrade has been refused (for appropriate reasons, like requested version and current version are identical), further upgrade requests will fail silently until agent is rebooted.
- **Bug Fix:** Termination tasks duplicates would end up being queued and use more resources than necessary.
- **Bug Fix:** `remote error: tls: bad certificate` when the registry has a self-signed certificate.
- **Bug Fix:** `api/v1/node` `ram` key/value returned the wrong GB value.
- **Bug Fix:** Pushing to the registry would cause the Templates list in the Controller UI to fail to load for the duration of the push.
- **Improvement:** More detailed logs for situations when the reserve space threshold is triggered.
- **Improvement:** Various security patches.
- **WARNING:** SHA-1 certificate support has been removed for TLS/HTTPS and Cert Auth. You will need to regenerate your certs.
- (Standalone Registry: 1.24.0-261d9df2)

### 1.23.0 (1.23.0-3523c787) - Apr 19th, 2022

- **Bug Fix:** VM Capacity reverts after rebooting the node.
- **Bug Fix:** Retrying a registry push too fast will cause a failure.
- **Bug Fix:** Controller UI was reporting inaccurate sizes for disk usage.
- **Improvement:** Older CMD logs are now rotated and cleaned up once the aggregate size of logs reaches 700MB (unless otherwise specified).
- **Improvement:** Anka 3 (apple processor) Nodes are forced to a capacity of 2 (Apple's limitation) to prevent confusion.
- **New Feature:** [Request a VM with a specific VLAN & View available VLANs attached to a specific Host/Anka Node]({{< relref "whats-new/build-cloud-1.23.0/index.md" >}}) (REST API ONLY)
- (Standalone Registry: 1.23.0-f21df6a2)


### 1.22.0 (1.22.0-5dc750f1) - Jan 10th, 2022

- **Bug Fix:** Available Node and Template information will now show properly for failed instances.
- **New Feature:** [VM Templates, Instances, and Nodes will all show the architecture (intel or arm) they support.]({{< relref "whats-new/build-cloud-1.22.0/index.md" >}})
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
- New Feature: [Delete VM Templates from Nodes through the Controller API.]({{< relref "whats-new/build-cloud-1.20.0/index.md#delete-vm-templates-on-node-from-controller-api" >}})
- New Feature: [Ability to see what VM Templates are on a node from the Controller API.]({{< relref "whats-new/build-cloud-1.20.0/index.md#view-availablepulled-vm-templates-on-a-node-from-controller-api" >}})
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
- New Feature: Ability to use certs and username/password for controller -> etcd connections | [Documentation]({{< relref "whats-new/anka-legacy.md#ability-to-use-certs-and-usernamepassword-for-etcd-connections" >}})
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
- New Feature: `script_result` object is now returned from [Save Image Template API]({{< relref "anka-build-cloud/working-with-controller-and-API.md#list-save-template-image-requests" >}})
- Bug Fix: The start vm queue code sometimes check tasks from the controller queue. It doesn't take the tasks but it can create load on the db
- (Standalone Registry: 1.15.0-a88b7bc)

---

### 1.14.0 (1.14.0-17620328) - Feb 18th, 2021
- New Feature: [A button will now show in the Controller UI allowing you to delete an Offline node (instead of having to issue an API call)]({{< relref "whats-new/anka-legacy.md#delete-button-will-show-for-offline-nodes" >}})
- New Feature: [You can now set a specific range of MAC addresses that are assigned to VM instances]({{< relref "whats-new/anka-legacy.md#customize-the-range-of-mac-addresses-the-controller-uses-for-creating-vms" >}})
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
- New Feature: [Unresponsive VM monitoring with `ankacluster join --enable-vm-monitor`]({{< relref "whats-new/anka-legacy.md#unresponsive-vm-monitor" >}})

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
- New Feature: [Controller Instances page now allows users to add custom columns (from metadata key/value)]({{< relref "whats-new/anka-legacy.md" >}})
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
