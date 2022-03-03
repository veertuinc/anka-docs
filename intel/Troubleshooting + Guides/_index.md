---
title: "Troubleshooting + Guides"
linkTitle: "Troubleshooting + Guides"
weight: 6
hideSubPagesInLeftMenu: true
showBaseDirName: true

---


{{< hint error >}}
EOL and Deprecation list:
- Anka Virtualization + CLI < 2.0 are EOL and no longer supported.
- Anka Build Cloud Controller and Registry versions < 1.8
- Jenkins Plugin versions < 2.0
- Anka GitLab Runner versions < 1.0
{{< /hint >}}

{{< hint info >}}
We recommend using our diagnostic package collection script. This script collects logs and usage statistics from the host and archives them.

```bash
curl https://raw.githubusercontent.com/veertuinc/scripts/main/generate-anka-node-diagnostics.bash -O && \
chmod +x generate-anka-node-diagnostics.bash && \
./generate-anka-node-diagnostics.bash
```

It is also best to run this as close to when an issue happens as several commands grab a window in time.
{{< /hint >}}

**Once you have collected the logs, go through the below mentioned troubleshooting steps and then contact support.**

---

## Anka Virtualization

### Troubleshooting checklist

- Check `df -h` and other host or VM resource usage (CPU/RAM) and be sure that the host resources are not exhausted.
- Ensure that the user running Anka VMs is logged in and that Sleep and passworded Screensaver are disable. The hypervisor will not work without an active/logged in user.
- Check `[~]/Library/Logs/Anka/` logs for any indication of why the failure happened.
- Ensure that all components in your Anka environment can communicate. This includes connectivity between CI/CD tooling we do not support.
- Disable anti-virus and firewalls on the host.

### Logs

{{< hint warning >}}
Anka runs as any user on the host, so the paths and locations described below can sometimes be under `$HOME/~`.
{{< /hint >}}

- Anka CLI commands executed as root/sudo: **`/Library/logs/anka`**
- Anka CLI commands as a non-root user: **`$HOME/Library/logs/anka`**

{{< hint info >}}
The above logs are rotated at 1MB and maintains a maximum of 10 files. This is not configurable.
{{< /hint >}}

In these directories you will find the following logs:

1. `anka.log` - The primary log including anka commands (Anka CLI, Anka run) STDOUT and STDERR

2. `lupd.log` - License auto-upgrade service logs; it's rarely used and most likely not related to VMs runtime

3. `ankanetd.log` - Anka VM network service logs; useful for troubleshooting VM networking related problems

4. `{UUID}.log` - (`{UUID}` is the specific VM's UUID) Specific VM logs; useful if VM exits abnormally or fails to start.

{{< hint info >}}
If you're using the Anka Build Cloud Controller to start and terminate VMs, you need to set an ENV so the Node Agent doesn't delete the `{UUID}.log` on termination. This requires editing the agent's plist:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>EnvironmentVariables</key>
    <dict>
        <key>ANKA_DELETE_LOGS</key>
        <string>0</string>
    </dict>
    <key>Label</key>
    <string>com.veertu.anka.cluster.agent</string>
    <key>ProgramArguments</key>
    <array>
      <string>/Library/Application Support/Veertu/Anka/bin/anka_agent_helper</string>
      <string>--controller-addr</string>
```
{{< /hint >}}

5. `/var/log/install.log` - Install related STDOUT/ERR

Anka and Apple will also report crashes in `/Library/Logs/DiagnosticReports` as `.crash` or `.hang`.

## Anka Build Cloud Controller / Registry

### Troubleshooting checklist

- Check `df -h` + CPU/RAM and be sure that the host resources are not exhausted.
- Look over the controller, registry, and even agent logs on the Anka Nodes and ensure that there are no configuration errors or clear indications of what is wrong.
- Ensure that all components in your Anka environment can communicate. This includes connectivity between CI/CD tooling we do not support.

### Logs

{{< hint info >}}
You might be interested in changing the verbosity of the following logs. If so, you can set the `ANKA_LOG_LEVEL` ENV to an integer greater than 0 and restart the controller. See [our Configuration Reference]({{< archRelRef "Anka Build Cloud/configuration-reference.md#logging" >}}) for more details.
{{< /hint >}}

#### Linux / Docker Package

Anka controller and registry services can run with linux, using docker containers. Logs are available via the controller dashboard, docker logs, and via a central-logs directory stored in the registry. Generally, log files are created for each vm upon vm start.

{{< hint info >}}
The Anka controller is responsible for cleaning unused logs.
{{< /hint >}}

##### Anka Controller

1. By Docker logs command : `docker logs --follow <Name of the container running the controller> ` 
 
2. The controller is an API, so all API connections made to it from Anka-agent or CI platforms(Jenkins) logs  in these logs. If a vm fails to start it suggests first to check this logs.

##### Anka Registry

Location : the directory you spcified in the docker-compose.yml, for mounting the logs to your machine. 

1. By docker logs command : `docker logs --follow <Name of the container    running the registry> `

2. the registry and agent logs share the same file. you can see it in Controller dashboard at the 'log' tab under the name of your host.

![agent logs]({{< siteurl >}}images/anka-build/logs/dashboardlogs.png)

----

#### Mac Package

{{< hint warning >}}
All logs larger than 1024MB older than 7 days are deleted every day (hard coded; no logrotate configuration)
{{< /hint >}}

Anka logs are available via the controller dashboard and several directories in correspondence with 'Anka' and it's microservices (controller, registry and agent). Generally, log files are created for each vm upon vm start.  
Anka controller is responsible for cleaning unused vms logs.

* anka Agent

##### Anka Controller

Logs location : `/Library/Logs/Veertu/AnkaController`
1. Show logs by command: `sudo anka-controller logs` - Press Ctrl+C to exit.
 
2. There are 4 types of log files, in the snapshot you can see log files **without** ID, they are **LINK** files- point to the latest log been created ( the last active vm) , each vm can generate all of the log types below. the robosety of the logs are from highest(INFO) to the lowest(ERROR), you can check this files using 'tail' command:

 * `anka-controller.INFO` - contains all of the below. 
 * `anka-controller.WARNING` - contains WARNNIGS & ERRORS.
 * `anka-controller.ERROR` - contains just ERRORS.
 * `anka_agent.FATAL` - Only FATAL ERRORS (both controller and agent).

![controller logs]({{< siteurl >}}images/anka-build/logs/ankaControllerlogs.png)


3. The controller is an API, so all the communication made from Anka-agent or CI platforms(Jenkins) stored in the controller logs. If a vm fails to start it suggests first to check this logs.


##### Anka Registry and Agent (available on your Anka Nodes)

Logs location : `/var/log/veertu`

1. Anka agent and registry share the same log file format, just with a slightly different name: `anka_agent/regd.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`
2. There are 4 type of LINK files, they hold the latest active vm logs, the verbose of the logs are from highest (INFO) to the lowest (ERROR), you can check this files using 'tail' command:

 * `anka_agent.INFO` - contains all of the below except for CMD log.
 * `anka_agent.WARNING` - contains WARNNIGS & ERRORS.
 * `anka_agent.ERROR` - contains just ERRORS.
 * `anka_agent.FATAL` - only FATAL ERRORS.
 * `anka_agent.CMD` - (new in 1.20.0) contains the various anka commands the agent is executing on the host as well as the returned data.

3. `vlaunchd.log`: Holds communication logs between registry **<--->** agent **--->** controller.
4. You can also read and download the agent and registry logs via the UI in the Controller dashboard.

![agent logs]({{< siteurl >}}images/anka-build/logs/dashboardlogs.png)

#### Centralized Logs

If `ANKA_ENABLE_CENTRAL_LOGGING` is set to true in your controller and registry config, logs for all Build Cloud services will be stored in the registry's data directory under `{registryStorageLocation}/files/central-logs/`.

You will find historical and current anka_agent logs from each of your nodes, the controller logs, and if applicable the registry logs.

- On macOS: The default is `/Library/Application\ Support/Veertu/Anka/registry/files/central-logs/`

- On docker: You set the storage location in your docker-compose.yml, but it will still be under `.../files/central-logs/`

---

## Troubleshooting Guides