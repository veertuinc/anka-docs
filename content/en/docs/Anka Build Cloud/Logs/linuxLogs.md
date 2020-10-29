---
title: "Linux logs"
linkTitle: "Linux logs"
weight: 3
description: >
---


## OVERVIEW

> All logs older than 7 days are deleted every day (hard coded; no logrotate configuration)

Anka controller and registry services can run with linux, using docker containers. logs are available via the controller dashboard , docker logs and via directories in correspondence with anka services. Generally, log files are created for each vm upon vm start .   
Anka controller is responsible for cleaning unused vms logs. 

### Anka Controller 

1. By Docker logs command : `docker logs --follow <Name of the container running the controller> ` 
 
2. The controller is an API, so all API connections made to it from Anka-agent or CI platforms(Jenkins) logs  in these logs. If a vm fails to start it suggests first to check this logs.

### Anka Registry

Location : the directory you spcified in the docker-compose.yml, for mounting the logs to your machine. 

1. By docker logs command : `docker logs --follow <Name of the container    running the registry> `

2. the registry and agent logs share the same file. you can see it in Controller dashboard at the 'log' tab under the name of your host.

![agent logs](/images/anka-build/logs/dashboardlogs.png)