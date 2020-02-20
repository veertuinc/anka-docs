---
title: "Mac logs"
linkTitle: "Mac logs"
weight: 3
description: >
---


## OVERVIEW

Anka logs are available via the controller dashboard and several directories in correspondence with 'Anka' and it's microservices (controller, registry and agent). Generally log files are created for each vm upon vm start.  
Anka controller is responsible for cleaning unused `vms logs. 

### Anka Controller 

Dir : `/Library/Logs/Veertu/AnkaController`
1. By command: `sudo anka-controller logs` - Press Ctrl+C to exit. The log level is a number starting with 0 as the lowest, the higher the log level means more verbose. The default log level is 0. 
 
2. There are 4 type of LINK files, they hold the lastest active vm logs , the robosety of the logs are from highest(INFO) to the lowest(ERROR), you can check this files using 'tail' command:
 * anka_agent.INFO
 * ana_agent.WARNING
 * anka_agent.ERROR
 * anka_agent.FATAL

3. The controller is an API, so all API connections made to it from Anka-agent or CI platforms(Jenkins) logs  in these logs. If a vm fails to start it suggests first to check this logs.

### Anka agent 

Dir : `/var/log/veertu`
1. Anka agent and registry share the same dir.
2. 4 types of LINK files as Controller logs above.
3. You can see the agents log via the logs screen in the controller dashboard it will be under the name of your Host.

### Anka Registry

Dir : `/var/log/veertu`
1. The registry and agent are in the same folder 
2. Holds communication logs between registry **<--->** agent **--->** controller and registry

### Ankacnt logs

 Dir : `/Library/logs/anka` || `$HOME/Library/logs/anka` (if using anka on root or user)
This logs are of the Anka program itself, in this dir you will find the following logs:

1. `anka.log` - Activity with anka commands(Anka CLI, Anka run) is logged there, also this log is not being removed but round robined

2. `Opd.log` - is from license autoupgrade service, it's rarely used and most likely not related to VMs runtime.

3. `ankanetd.log` - the  network service, could be analyzed if some error related to network are reported.

4. `UUID.log` - UUID is the VM's uuid - it's VM log, usually we check these logs when VM exits abnormally or fails to start.


## Controller API activity logging ( Enterprise Plus only) :

This is an automated event log tool which exports logs to your chosen service.  so you can follow the periodic activity of your vms. The controller will send events in json format when certain actions take place.
[The full list of events and their properties](https://docs.google.com/spreadsheets/d/1QA7u4hIi1V1kVvNxffFHOWjYsc7XKNFLyYnp31EVLKc/edit#gid=0)

### Usage 

### Using POST request 

Events are sent via http POST to the registry.
The user can override the url that the request are sent to with the --event-log-url parameter.
All the events will be sent in http POST requests in json format

#### Using EventSource

Events can be pulled in real time from the registry using http server-sent-events or EventSource.
The eventsource is served from the /log path on the registry.
Example:
https://<registry_host>:<port>/log?service=event
You can see a working example when you click the stream button in the logs section of the ui




## Anka Registry Docker container
### Default Ports
8089
### Binaries and scripts
`/usr/bin/anka-registry`

### Configuration files
Configuration is done through docker-compose file or through environment variables.

### Logs
Logs will be written to:  
`/var/log/anka-registry`  
It's also possible to get the logs through `docker logs` command.  
Assuming your container is called `anka-registry-1`:  
```shell
docker logs --follow anka-registry-1
```

### Data
Registry data will be written to:
`/mnt/vol`


## Anka Controller Deb package
### Default Ports
80
### Binaries and scripts
Application Binary:
`/usr/bin/anka-controller`
Start/Stop daemon:  
`/etc/init.d/anka-controller`
### Configuration files
`/etc/anka-controller/controller.conf`

### Logs
`/var/log/anka-controller`

### Data
No data is saved.

## Anka Registry Deb package
### Default Ports
80
### Binaries and scripts
Application Binary:  
`/usr/bin/anka-registry`  
Start/Stop daemon:  
`/etc/init.d/anka-registry`  

### Configuration files
`/etc/anka-registry/reg.conf`

### Logs
`/var/log/anka-registry`

### Data
Data will be written to:  
`/var/anka`

1. Configuration mistakes
2. SSH port forwarding not set correctly on VM 
3. JNLP port is not accessible from VM
4. Outdated Jenkins environment
5. VM has different version of Java from the Master
6. The Anka Cloud can't run VMs (see [VM is stuck at scheduling]({{< relref "vm-stuck-scheduling.md">}}))
