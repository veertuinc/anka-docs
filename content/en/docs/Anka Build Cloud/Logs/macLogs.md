---
title: "Mac logs"
linkTitle: "Mac logs"
weight: 3
description: >
---


## OVERVIEW

Anka logs are available via the controller dashboard and also in several directories in Anka and controller, registry and agent modules. Generally, log files are created for each vm upon vm start.  
Anka controller is responsible for cleaning unused vms logs. 

### Anka Controller 

Logs location : `/Library/Logs/Veertu/AnkaController`
1. Show logs by command: `sudo anka-controller logs` - Press Ctrl+C to exit. The log level is a number starting with 0 as the lowest, the higher the log level means more verbose. The default log level is 0. 
 
2. There are 4 types of log files, in the snapshot you can see logs **without** ID, they are **LINK** files- pointing to the latest log created ( the last active vm). Each vm can generate all of the log types below. The verbosity of the logs are from highest(INFO) to the lowest(ERROR). You can check these files using the 'tail' command:

 * anka-controller.INFO - contains all of the below. 
 * anka-controller.WARNING - contains WARNNIGS & ERRORS.
 * anka-controller.ERROR - contains just ERRORS.
 * anka_agent.FATAL - Only FATAL ERRORS (both controller and agent).

![controller logs](/images/anka-build/logs/ankaControllerlogs.png)


All the communication made from Anka-agent or CI platforms(Jenkins) to controller REST APis is stored in the controller logs. If a VM fails to start, look in the controller logs.

### Anka Registrey and Anka Agent 

Logs location : `/var/log/veertu`
1. Anka agent and registry share the same log files.
 `anka_agent.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`
2. 4 types of LINK files as Controller logs above.
3. You can see the agent and registry logs via the UI in the controller dashboard it will be under the name of your host. 
4. These logs contain communication logs between registry **<--->** agent **--->** controller. 

![agent logs](/images/anka-build/logs/dashboardlogs.png)


### Anka logs

Logs location : `/Library/logs/anka` || `$HOME/Library/logs/anka` (if using anka on root or user)
These logs are of the Anka binarey itself. 
You will see the following logs:

![ankabinary logs](/images/anka-build/logs/ankabinarylogs.png)

1. `anka.log` - Activity with anka commands(Anka CLI, Anka run) is logged there. This log is not delted but refreshed with latest activity in a round robin manner.

2. `Opd.log` - is from license autoupgrade service, it's rarely used and most likely not related to VMs runtime.

3. `ankanetd.log` - is for the  network service, should be analyzed if some error related to network are reported.

4. `UUID.log` - UUID is the VM's uuid - it's the VM log. Check this logs when VM exits abnormally or fails to start.


**Best practice  - Start troubleshooting the controller logs first, then move on to Anka Agent logs and then to Anka Binary logs.**
