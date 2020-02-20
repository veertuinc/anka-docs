---
title: "Mac logs"
linkTitle: "Mac logs"
weight: 3
description: >
---


## OVERVIEW

Anka logs are available via the controller dashboard and several directories in correspondence with 'Anka' and it's microservices (controller, registry and agent). Generally, log files are created for each vm upon vm start.  
Anka controller is responsible for cleaning unused vms logs. 

### Anka Controller 

Logs location : `/Library/Logs/Veertu/AnkaController`
1. Show logs by command: `sudo anka-controller logs` - Press Ctrl+C to exit. The log level is a number starting with 0 as the lowest, the higher the log level means more verbose. The default log level is 0. 
 
2. There are 4 types of log files, in the snapshot you can see logs **without** ID, they are **LINK** files- point to the latest log been created ( the last active vm) , each vm can generate all of the log types below. the robosety of the logs are from highest(INFO) to the lowest(ERROR),, you can check this files using 'tail' command:

 * anka-controller.INFO - contains all of the below. 
 * anka-controller.WARNING - contains WARNNIGS & ERRORS.
 * anka-controller.ERROR - contains just ERRORS.
 * anka_agent.FATAL - Only FATAL ERRORS (both controller and agent).

![controller logs](/images/anka-build/logs/ankaControllerlogs.png)


3. The controller is an API, so all the communication made from Anka-agent or CI platforms(Jenkins) stored in the controller logs. If a vm fails to start it suggests first to check this logs.


### Anka Registrey and Agent 

Logs location : `/var/log/veertu`
1. Anka agent and registry share the same log files.
 `anka_agent.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`
2. 4 types of LINK files as Controller logs above.
3. You can see the agents and registry logs via the UI in the controller dashboard it will be under the name of your host. 
4. Holds communication logs between registry **<--->** agent **--->** controller. 

![agent logs](/images/anka-build/logs/dashboardlogs.png)


### Anka logs

 Logs location : `/Library/logs/anka` || `$HOME/Library/logs/anka` (if using anka on root or user)
This logs are of the Anka binarey itself, in this dir you will find the following logs:

![ankabinary logs](/images/anka-build/logs/ankabinarylogs.png)

1. `anka.log` - Activity with anka commands(Anka CLI, Anka run) is logged there, also this log is not being removed but round robined

2. `Opd.log` - is from license autoupgrade service, it's rarely used and most likely not related to VMs runtime.

3. `ankanetd.log` - the  network service, could be analyzed if some error related to network are reported.

4. `UUID.log` - UUID is the VM's uuid - it's VM log, usually we check these logs when VM exits abnormally or fails to start.


**Best practice for troubleshooting with the logs is to start from Controller down to Anka binary**