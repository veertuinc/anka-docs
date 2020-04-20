---
title: "Anka Build Cloud on Mac OS"
linkTitle: "Anka Build Cloud on Mac OS"
weight: 1
description: >
  Set up your Anka Build Cloud on Mac OS.
---

Welcome! This tutorial will guide you through setting up your Anka Build Cloud.

## Necessary Hardware

1. A Mac to install the Anka Controller & Registry.
2. A second Mac to install the Anka CLI (the "Node").

> You can complete this tutorial with only one machine running Mac OS, but it's not recommended.

{{< include file="shared/content/en/docs/Getting Started/partials/_what-we-are-doing.md" >}}

## Step 1: Get familiar with the Anka CLI

### Install the Anka CLI

{{< include file="shared/content/en/docs/Anka CLI/partials/_install-guide.md" >}}

For Anka CLI commands and options, see the [Command Reference]({{< relref "docs/Anka CLI/command-reference.md" >}}).

### Create your first VM Template

{{< include file="shared/content/en/docs/Getting Started/partials/_create-vm-template.md" >}}

> You can find detailed instructions for [`anka create`]({{< relref "docs/Anka CLI/command-reference.md#create" >}}) [here.]({{< relref "docs/Anka CLI/creating-templates-and-tags.md" >}})

> You can continue on to Step 2 while you wait for this to finish.

## Step 2. Install Anka Controller & Registry

> Perform the following steps on the machine intended to run the Controller & Registry.

### Download the Controller & Registry PKG

Download the file called "Cloud Controller & Registry (Run on Mac)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}
If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -S -L -o ~/Downloads/AnkaControllerRegistry.pkg https://veertu.com/downloads/ankacontroller-registry-mac-latest
```

### Install the Controller & Registry PKG

Double click on the .pkg to start the UI install process.
- Or, you can install the package using the command line:
    ```shell
    sudo installer -pkg ~/Downloads/AnkaControllerRegistry.pkg -target /
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

{{< include file="shared/content/en/docs/Getting Started/partials/_controller-listening-on-80-and-orientation.md" >}}

#### Configuration and scripts ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference))

The Anka Controller **AND Registry** command is installed into `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges:
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
When `sudo anka-controller start` is executed, the script will use `launchd` to load the daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`.
 - The Anka Controller & Registry run script is `/usr/local/bin/anka-controllerd`. This file acts as a run script **and configuration file**. You can modify it to change the default ports used by adding the proper option or ENV. For example, if you want to run the Registry on a different port and use 127.0.0.1, you would add the following above the `"$CONTROLLER_BIN"` line ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference)): 
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
 
> You can modify the destination in the `/usr/local/bin/anka-controllerd` file ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference/#logging)).

You can also watch the logs live (similar to tail -f):
```shell
sudo anka-controller logs
```
 
> The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference/#logging))

{{< include file="shared/content/en/docs/Getting Started/partials/_step3-and-4.md" >}}

## What next?

- Browse the [Anka CLI Command Reference]({{< relref "docs/Anka CLI/command-reference.md" >}}).  
- Connect your cloud to a [CI server]({{< relref "docs/Anka Build Cloud/CI Plugins/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "docs/Anka Build Cloud/controller-api.md">}}).  
- Learn how to work with [USB devices]({{< relref "docs/Getting Started/working-with-usb-devices.md">}})