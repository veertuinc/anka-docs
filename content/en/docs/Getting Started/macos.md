
---
title: "Getting started with Anka Build Cloud On Mac OS"
linkTitle: "Run Anka Build Cloud on Mac OS"
weight: 2
description: >
  Set up your Anka Build Cloud on Mac OS.
---


Welcome! this tutorial will guide you through setting up your Anka Build Cloud on Mac OS.

## Necessary hardware 

1. Mac running Mac OS to install Anka Controller + Registry.
2. At least one Mac running Mac OS to install Anka CLI as a Node.

You can complete this tutorial with only one machine running Mac OS, but it's less recommended. 

## What we will do in this tutorial
1. Create your first VM
2. Install Anka Controller and Registry
3. Add one or more Macs as Nodes
4. Start a VM instance using the Anka Build Cloud web interface

## Step 1. Create your first VM



**Perform the following steps on the machine that is intended to be the Node.**


{{< include file="shared/content/en/docs/Getting Started/partials/_first-vm.md" >}}
For more commands and options, see [Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).
While you wait for the VM creation, continue and perform step 2.

## Step 2. Install Anka Controller and Registry

**Perform the following steps on the machine intended to run the Controller and Registry**

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


Great! now that we have our Anka Controller and Registry up and running let's add Nodes!


## Step 3. Add Nodes

**Perform this on the machine you created your first VM on**


{{< include file="shared/content/en/docs/Getting Started/partials/_push-to-registry.md" >}}

After the push is finished you should be able to see your new Template in the "templates" section.

![Your first template](/images/getting-started/push-template.png)


{{< include file="shared/content/en/docs/Getting Started/partials/_add-node.md" >}}

**Repeat this process on other machines that you want to join as Nodes (Anka CLI needs to be installed)**


## Step 4. Start a VM instance using the Anka Dashboard


{{< include file="shared/content/en/docs/Getting Started/partials/_start-vm-dash.md" >}}



## Where to go next?

Browse [Anka CLI Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).  
Connect your cloud to a [CI server]({{< relref "docs/Anka Build Cloud/CI Plugins/_index.md" >}}).  
Find out how to use the [Controller REST API]({{< relref "docs/Anka Build Cloud/controller-api.md">}}).  
Learn how to work with [USB devices]({{< relref "docs/Anka Build Cloud/using-real-devices-attached-to-anka-vms.md">}})


