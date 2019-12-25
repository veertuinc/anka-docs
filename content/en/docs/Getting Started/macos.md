
---
title: "Getting started with Anka Build Cloud On Mac OS"
linkTitle: "Run Anka Build Cloud on Mac OS"
weight: 2
draft: true
description: >
  Set up your Anka Build Cloud
---


Welcome! this tutorial will guide you through setting up your Anka Build Cloud on Mac OS.

## Necessary hardware 

1. Mac running Mac OS to install Anka Controller + Registry.
2. At least one Mac running Mac OS to install Anka CLI as a Node.

You can complete this tutorial with only one machine running Mac OS, but it's less recommended. 

## What we will do in this tutorial
1. Install Anka Controller and Registry
2. Install Anka CLI on one or more Mac(s)
3. Create your first VM
4. Start a VM instance using the Anka Build Cloud web interface



## Install Anka Controller and Registry
Anka Controller and Registry can be installed on Mac OS or on Linux.
### Download the package
Download the file called "Cloud Controller & Registry (Run on Mac)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page" >}}.
If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -L -o AnkaControllerRegistry-1.5.2-ce0d3271.pkg https://veertu.com/downloads/ankacontroller-registry-mac-latest
```
The file name is constructed like this - `AnkaControllerRegistry-VERSION-HASH.pkg`. We follow semantic versioning so the version number is divided to `MAJOR`, `MINOR` and `PATCH`. 

### Install
For UI installation - just open the file you downloaded and follow the instructions.
Or install the package using the command line - 
```shell
sudo installer -pkg AnkaControllerRegistry-1.5.2-ce0d3271.pkg -target /
```

### Verify your installation
Using the command line, execute the controller status command:
```shell
sudo anka-controller status
```
The response should be:
```shell
Anka Controller is Running
```
Anka Controller should be listening on port 80 (http). Try pointing your browser to the machine's ip or host name. You can use `localhost` or `127.0.0.1` when using the machine the server is installed on.

Your new dashboard should look like the picture below

![How your new dashboard looks like](/images/getting-started/new-dashboard.png)


### Orientation
#### Running Applications
Let's take a look on what is now running on your machine.
1. Anka Controller serving web UI and REST API on port 80.
2. Anka Registry serving REST API on port 8089.
3. ETCD database server serving on ports 2379 and 2380. This server is used internally by the Anka Controller
#### Configuration and scripts
Anka Controller and Registry start-stop executable script is at `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges.
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
Anka Controller and Registry run script is at `/usr/local/bin/anka-controllerd`. This file acts as a run script and configuration file. 
Launchd Daemon is at `/Library/LaunchDaemons/com.veertu.anka.controller.plist`. When `sudo anka-controller start` is executed the script will use `launchd` to load to daemon. The daemon will then call `/usr/local/bin/anka-controllerd`.
#### Logs
Logs are written to `/Library/Logs/Veertu/AnkaController` by default. 
You can view the INFO log using tail:
```shell
tail -f /Library/Logs/Veertu/AnkaController/anka-controller.INFO
```
Press Ctrl+C to exit. 

You can also view logs using an `anka-controller` command:
```shell
sudo anka-controller logs
```
Press Ctrl+C to exit.
The log level is a number starting with 0 as the lowest, the higher the log level means more verbose. The default log level is 0.

### Next steps
Great, now that we have our Anka Controller and Registry up and running 








