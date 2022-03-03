---
title: "Setting up on macOS"
linkTitle: "Setting up on macOS"
weight: 2
description: >
  Set up your Anka Build Cloud on macOS
---

Welcome! This tutorial will guide you through setting up your Anka Build Cloud.

{{< hint warning >}}
**This guide is for first time users and shouldn't be used for upgrading. Please use [the upgrading guide]({{< relref "arm/Anka Build Cloud/upgrading.md" >}}) instead.**
{{< /hint >}}

{{< hint info >}}
The macOS package will install and run using Apple's Rosetta. There is no native arm package at this time.
{{< /hint >}}

## Necessary Hardware

1. A Mac to install the Anka Controller & Registry.
2. A second Mac to install the Anka CLI (the "Node").

> You can complete this tutorial with only one machine running Mac OS, but it's not recommended.

{{< include file="_partials/intel/Getting Started/_what-we-are-doing.md" >}}

## Step 1: Get familiar with the Anka CLI

### Install the Anka Virtualization package

#### Download and install with your terminal

{{< include file="_partials/intel/Anka Virtualization/_download-and-install-with-terminal.md" >}}

#### Verify the installation

{{< include file="_partials/intel/Anka Virtualization/_verify-installation.md" >}}

For Anka CLI commands and options, see the [Command Reference]({{< relref "arm/Anka Virtualization/command-reference.md" >}}).

### Obtain the macOS installer

{{< include file="_partials/intel/Getting Started/_obtain-macos-installer.md" >}}

### Create the VM Template

#### Using the Anka UI

#### Using the Anka CLI

{{< include file="_partials/intel/Anka Virtualization/create/_example.md" >}}

> You can find detailed instructions for [`anka create`]({{< relref "arm/Anka Virtualization/command-reference.md#create" >}}) [here.]({{< relref "arm/Getting Started/creating-your-first-vm.md" >}})

> You can continue on to Step 2 while you wait for this to finish.

## Step 2. Install the Anka Build Cloud Controller & Registry

> Perform the following steps on the machine intended to run the Controller & Registry.

### Download the Controller & Registry PKG

Download the file called "Cloud Controller & Registry (Run on Mac)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}
If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-mac-latest) | cut -d/ -f5) https://veertu.com/downloads/ankacontroller-registry-mac-latest
```

### Install the Controller & Registry PKG

Double click on the .pkg to start the UI install process.
- Or, you can install the package using the command line:
    ```shell
    sudo installer -pkg ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-mac-latest) | cut -d/ -f5) -target /
    ```

### Verify your installation
Two methods are available:
- Use the CLI status command:
  ```shell
  sudo anka-controller status
  ```
- Use curl:
  ```shell
  curl http://<ip>:8080/api/v1/status
  ```

{{< include file="_partials/intel/Getting Started/_controller-listening-on-80-and-orientation.md" >}}

#### Configuration and scripts ([reference]({{< relref "arm/Anka Build Cloud/configuration-reference.md" >}}))

The Anka Controller **AND Registry** command is installed into `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges:
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
When `sudo anka-controller start` is executed, the script will use `launchd` to load the daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`.
 - The Anka Controller & Registry run script is `/usr/local/bin/anka-controllerd`. This file acts as a run script **and configuration file**. You can modify it to change the default ports used by adding the proper option or ENV. For example, if you want to run the Registry on a different port and use 127.0.0.1, you would add the following above the `"$CONTROLLER_BIN"` line ([reference]({{< relref "arm/Anka Build Cloud/configuration-reference.md" >}})): 
    ```shell
    export ANKA_ANKA_REGISTRY="http://127.0.0.1:8081"
    export ANKA_REGISTRY_LISTEN_ADDRESS=":8081" 
    ```

#### Logging

Logs are written to `/Library/Logs/Veertu/AnkaController` by default:
```shell
/Library/Logs/Veertu/AnkaController/anka-controller.INFO
/Library/Logs/Veertu/AnkaController/anka-controller.WARNING
/Library/Logs/Veertu/AnkaController/anka-controller.ERROR
```
 
> You can modify the destination in the `/usr/local/bin/anka-controllerd` file ([reference]({{< relref "arm/Anka Build Cloud/configuration-reference.md#logging" >}})).

You can also watch the logs live (similar to tail -f):
```shell
sudo anka-controller logs
```
 
> The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "arm/Anka Build Cloud/configuration-reference.md#logging" >}}))

## Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

> Perform the following steps on the Node where you created your first VM Template.

### Add the Registry

{{< include file="_partials/intel/Getting Started/_add-the-registry.md" >}}

### Push the VM to the Registry

{{< include file="_partials/intel/Getting Started/_push-vm-to-registry.md" >}}

### Join to the Controller & Registry

{{< include file="_partials/intel/Getting Started/_join-to-cluster.md" >}}

## Step 4: Start a VM instance using the Controller UI

{{< include file="_partials/intel/Getting Started/_start-vm-instance-using-controller-ui.md" >}}

## Step 5: Orientate to your new environment

### Anka Controller and Registry package

- Default Ports: Anka Controller: 80 / Anka Registry: 8089  
- Binaries and scripts: Anka Controller has only one combined binary. It can run Anka Controller, Anka Registry and ETCD Server.  
    Installed at: `/Library/Application Support/Veertu/Anka/bin/anka-controller`  
    Start script: `/usr/local/bin/anka-controllerd`  
    Start/Stop script: `/usr/local/bin/anka-controller`  
    Launchd daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`
- Configuration files: Configuration for this package is done by altering the start script at `/usr/local/bin/anka-controllerd`.
- Logs: `/Library/Logs/Veertu/AnkaController`
- Data:
    ETCD data will be saved in: `/Library/Application Support/Veertu/Anka/anka-controller`  
    Registry data will be saved in: `/Library/Application Support/Veertu/Anka/registry`

### Standalone Anka Registry

- Default Ports: 80
- Binaries and scripts:
    Anka Registry for Mac binary is installed at: `/Library/Application Support/Veertu/Anka/bin/ankaregd`  
    Launchd daemon: `/Library/LaunchDaemons/com.veertu.anka.registry.plist`
- Configuration files: Configuration for this package is done by altering the Launchd daemon xml file at `/Library/LaunchDaemons/com.veertu.anka.registry.plist`. Be sure to unload it first.
- Data will be saved in: `/Library/Application Support/Veertu/Anka/registry`

---

## What next?

- [Prepare and join your machines to the Build Cloud]({{< relref "arm/Anka Build Cloud/preparing-and-joining-your-nodes.md" >}}).  
- Browse the [Anka CLI Command Reference]({{< relref "arm/Anka Virtualization/command-reference.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "arm/CI Plugins and Integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "arm/Anka Build Cloud/working-with-controller-and-api.md#rest-api">}}).  













