---
---

{{< hint error >}}
EOL and Deprecation list:
- Anka Build Cloud Controller and Registry versions < 1.8 EOL
{{< /hint >}}

**Once you have collected the logs, go through the below mentioned troubleshooting steps and then contact support.**

---

## Anka Build Cloud Controller / Registry

### Troubleshooting checklist

- Check `df -h` + CPU/RAM and be sure that the host resources are not exhausted.
- Look over the controller, registry, and even agent logs on the Anka Nodes and ensure that there are no configuration errors or clear indications of what is wrong.
- Ensure that all components in your Anka environment can communicate. This includes connectivity between CI/CD tooling we do not support.

### Logs

{{< hint info >}}
You might be interested in changing the verbosity of the following logs. If so, you can set the `ANKA_LOG_LEVEL` ENV to an integer greater than 0 and restart the controller. See [our Configuration Reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}}) for more details.
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

Anka logs are available via the Controller UI under Logs as well as several directories on the macOS machine hosting it.

##### Controller

- Logs location : `/Library/Logs/Veertu/AnkaController`

1. Show logs by command: `sudo anka-controller logs` - Press Ctrl+C to exit.

2. There are 4 types of log files, in the snapshot you can see log files **without** ID, they are **LINK** files- point to the latest log been created ( the last active vm) , each vm can generate all of the log types below. the robosety of the logs are from highest(INFO) to the lowest(ERROR), you can check this files using 'tail' command:

- `anka-controller.INFO` - contains ALL logs.
- `anka-controller.WARNING` - contains WARNINGS & ERRORS.
- `anka-controller.ERROR` - contains just ERRORS.
- `anka_agent.FATAL` - Only FATAL ERRORS (both controller and agent).

  ![controller logs]({{< siteurl >}}images/anka-build/logs/ankaControllerlogs.png)

{{< hint info >}}
The controller is an API, so all the communication made from Anka-agent or CI platforms(Jenkins) stored in the controller logs. If a vm fails to start it suggests first to check this logs.
{{< /hint >}}

{{< hint info >}}
The controller relies on an internal ETCD database. Logs for ETCD will be included in the controller logs, but by default they are set to be non-verbose.
{{< /hint >}}

##### Registry

- Logs location: `/var/log/veertu`

- Format: `regd.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`

There are 4 type of symlinks in the logs location pointing to the latest active logs. The verbosity of the logs are from highest (INFO) to the lowest (ERROR):

- `regd.INFO` - contains ALL logs.
- `regd.WARNING` - contains WARNINGS & ERRORS.
- `regd.ERROR` - contains just ERRORS.
- `regd.FATAL` - only FATAL ERRORS.

You can also read and download the logs via the UI in the Controller dashboard. Only relevant if you've [joined your Node to the Build Cloud Controller & Registry]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).

{{< include file="_partials/troubleshooting/node-agent.md" >}}

{{< include file="_partials/troubleshooting/centralized-logs.md" >}}

---
