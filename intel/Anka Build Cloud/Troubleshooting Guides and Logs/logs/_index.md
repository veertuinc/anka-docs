---
title: "Logs"
linkTitle: "Logs"
weight: 1
---

{{< hint info >}}
You might be interested in changing the verbosity of the following logs. If so, you can set the `ANKA_LOG_LEVEL` ENV to an integer greater than 0 and restart the controller. See [our Configuration Reference]({{< relref "intel/Anka Build Cloud/configuration-reference.md#logging" >}}) for more details.
{{< /hint >}}

## Linux / Docker Package

Anka controller and registry services can run with linux, using docker containers. Logs are available via the controller dashboard, docker logs, and via a central-logs directory stored in the registry. Generally, log files are created for each vm upon vm start.

{{< hint info >}}
The Anka controller is responsible for cleaning unused logs.
{{< /hint >}}

### Anka Controller

1. By Docker logs command : `docker logs --follow <Name of the container running the controller> ` 
 
2. The controller is an API, so all API connections made to it from Anka-agent or CI platforms(Jenkins) logs  in these logs. If a vm fails to start it suggests first to check this logs.

### Anka Registry

Location : the directory you spcified in the docker-compose.yml, for mounting the logs to your machine. 

1. By docker logs command : `docker logs --follow <Name of the container    running the registry> `

2. the registry and agent logs share the same file. you can see it in Controller dashboard at the 'log' tab under the name of your host.

![agent logs]({{< siteurl >}}images/anka-build/logs/dashboardlogs.png)

----

## Mac Package

{{< hint warning >}}
All logs larger than 1024MB older than 7 days are deleted every day (hard coded; no logrotate configuration)
{{< /hint >}}

Anka logs are available via the controller dashboard and several directories in correspondence with 'Anka' and it's microservices (controller, registry and agent). Generally, log files are created for each vm upon vm start.  
Anka controller is responsible for cleaning unused vms logs.

* anka Agent

### Anka Controller

Logs location : `/Library/Logs/Veertu/AnkaController`
1. Show logs by command: `sudo anka-controller logs` - Press Ctrl+C to exit.
 
2. There are 4 types of log files, in the snapshot you can see log files **without** ID, they are **LINK** files- point to the latest log been created ( the last active vm) , each vm can generate all of the log types below. the robosety of the logs are from highest(INFO) to the lowest(ERROR), you can check this files using 'tail' command:

 * `anka-controller.INFO` - contains all of the below. 
 * `anka-controller.WARNING` - contains WARNNIGS & ERRORS.
 * `anka-controller.ERROR` - contains just ERRORS.
 * `anka_agent.FATAL` - Only FATAL ERRORS (both controller and agent).

![controller logs]({{< siteurl >}}images/anka-build/logs/ankaControllerlogs.png)


3. The controller is an API, so all the communication made from Anka-agent or CI platforms(Jenkins) stored in the controller logs. If a vm fails to start it suggests first to check this logs.


### Anka Registry and Agent (available on your Anka Nodes)

Logs location : `/var/log/veertu`
1. Anka agent and registry share the same log files.
 `anka_agent.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`
2. There are 4 type of LINK files, they hold the latest active vm logs , the robosety of the logs are from highest(INFO) to the lowest(ERROR), you can check this files using 'tail' command:

 * `anka_agent.INFO` - contains all of the below except for CMD log.
 * `anka_agent.WARNING` - contains WARNNIGS & ERRORS.
 * `anka_agent.ERROR` - contains just ERRORS.
 * `anka_agent.FATAL` - only FATAL ERRORS.
 * `anka_agent.CMD` - (new in 1.20.0) contains the various anka commands the agent is executing on the host as well as the returned data.

4. `vlaunchd.log`: Holds communication logs between registry **<--->** agent **--->** controller.
3. You can see the agents and registry logs via the UI in the controller dashboard it will be under the name of your host. 

![agent logs]({{< siteurl >}}images/anka-build/logs/dashboardlogs.png)

---

### Centralized Logs

If `ANKA_ENABLE_CENTRAL_LOGGING` is set to true in your controller and registry config, logs for all Build Cloud services will be stored in the registry's data directory under `{registryStorageLocation}/files/central-logs/`.

You will find historical and current anka_agent logs from each of your nodes, the controller logs, and if applicable the registry logs.

- On macOS: The default is `/Library/Application\ Support/Veertu/Anka/registry/files/central-logs/`

- On docker: You set the storage location in your docker-compose.yml, but it will still be under `.../files/central-logs/`