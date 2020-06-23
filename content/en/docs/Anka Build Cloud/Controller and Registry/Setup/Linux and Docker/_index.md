
---
date: 2019-07-03T22:24:47-05:00
title: "Linux & Docker"
linkTitle: "Linux & Docker"
weight: 1
description: >
  Anka Build Cloud installation instructions for Linux & Docker
---

[//]: # (TODO: split this to files and add configuration )


## Controller/Registry Linux Application package
### Install
***Note*** You can run Controller/Registry docker containers on a Linux instance. We don't recommend that you run these containers on Mac hardware. If you want to run controller/registry on mac, then use the mac application package.

Move the `anka-controller-registry-XXX.tar.gz` tar file to a directory where you want the containers to run.

1. Uncompress the tar package.
2. Edit `config.yml` and enter values for the following : external_registry_address, data_mount_dir.
***Note*** default port settings - Controller on port 80 and registry on port 8089.
3. Run `./ankaCloudConfig docker fromFile`. This will update your docker-compose with configuration changes.
4. Execute `docker-compose up -d --build`
5. Execute `docker ps -a` to check if the anka controller and anka registry containers are up and running.
6. Go to controllerIP in your web browser and check to make sure that you can access the controller portal dashboard.


***Note*** If you want to change default port settings, don't edit `config.yml`. Edit the `advanced.yml`.

1. `advanced.yml` configuration settings to edit for custom ports - external_registry_address, listen_on, data_mount_dir.
2. Run `./ankaCloudConfig docker fromFile -i advanced.yml`
3. Execute `docker-compose up -d --build`

### Upgrade
First, Download the latest anka-controller-registry-XXX.tar.gz from [Anka Build Download page](https://veertu.com/download-anka-build/).  

Then, execute the following steps.  

1. Go to the controller/registry directory and execute `docker-compose stop`.  

2. Backup your existing `config.yml` and `advanced.yml`.  

3. Uncompress the new controller/registry tar package.  

4. Replace `config.yml` and/or `advanced.yml` with your backed up copies of these files.  

5. Execute `./ankaCloudConfig docker fromFile` and/or `./ankaCloudConfig docker fromFile -i advanced.yml`  

6. Execute `docker-compose up -d --build`

### Uninstall
1. Go to the controller/registry directory and execute `docker-compose stop`.
2. Delete the controller/registry directory.



## Debugging Issues with Controller

Anka controller is a server that usually runs as a daemon and errors are usually written to a log.
Check the output of `docker ps`. The anka controller container status should be something like this.

```
“Up for x amount of time”. For example:
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                    NAMES
ae6b522fdec4        test220919_anka-controller    "/bin/bash -c 'anka-…"   2 minutes ago       Up 2 minutes        0.0.0.0:8090->80/tcp     test220919_anka-controller_1
```

If the controller is crashing the status could be “restarting” or “failed”. Example for restarting state: “Restarting (1) 12 seconds ago”.

You can try to run `docker-compose up` without any parameters and this will output stdout for all services to the screen.

In case one of the services is crashing you could see the errors.Here is an example.
```
anka-controller_1   | E1128 15:17:16.952500       1 main.go:618] Could not build tls configuration:
anka-controller_1   | E1128 15:17:16.953204       1 main.go:619] open /whwhwhw/path: no such file or directory
test220919_anka-controller_1 exited with code 1
```
You can also take a look at the logs using `docker logs --follow $CONTAINER_NAME`.

