---
title: "Setting up the Controller & Registry on macOS"
toc_hide: true
weight: 1
---

Welcome! This tutorial guides you through setting up your Anka Build Cloud Controller & Registry on MacOS.

{{< hint warning >}}
**This guide is for first time users and shouldn't be used for upgrading. Please use [the upgrading guide]({{< relref "anka-build-cloud/upgrading.md" >}}) instead.**
{{< /hint >}}

## Checklist

- [ ] Set up the Anka Build Cloud components.
- [ ] Get Orientated.

## MacOS

{{< hint info >}}
The macOS package will install and run using Apple's Rosetta. There is no native arm package at this time.
{{< /hint >}}

### Necessary Hardware

1. A Mac to install the Anka Controller and/or Registry.
2. A second Mac to install the Anka CLI (the "Node").

> You can complete this tutorial with only one machine running Mac OS, but it's not recommended.

{{< include file="_partials/anka-build-cloud/_what-we-are-doing-mac.md" >}}

#### Download the Controller and/or Registry PKG

Download the file called "Cloud Controller & Registry (Run on Mac)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}
If you are more comfortable with the command line, you can download the file with curl:

```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-mac-latest) | cut -d/ -f5) https://veertu.com/downloads/ankacontroller-registry-mac-latest
```

#### Install the Controller and/or Registry PKG

Double click on the .pkg to start the UI install process. Or, you can install the package using the command line:

  ```shell
  sudo installer -pkg ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-mac-latest) | cut -d/ -f5) -target /
  ```

#### Verify your installation (macOS)

Two methods are available:
- Use the CLI status command:
  ```shell
  sudo anka-controller status
  ```
- Use curl:
  ```shell
  curl http://<ip>/api/v1/status
  ```

#### Orientation (macOS)

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

#### Configuration and Scripts

- [Config Reference]({{< relref "anka-build-cloud/configuration-reference.md" >}})

The Anka Controller **AND Registry** command is installed into `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges:
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
When `sudo anka-controller start` is executed, the script will use `launchd` to load the daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`.
 - The Anka Controller & Registry run script is `/usr/local/bin/anka-controllerd`. This file acts as a run script **and configuration file**. You can modify it to change the default ports used by adding the proper option or ENV. For example, if you want to run the Registry on a different port and use 127.0.0.1, you would add the following above the `"$CONTROLLER_BIN"` line ([reference]({{< relref "anka-build-cloud/configuration-reference.md" >}})): 
    ```shell
    export ANKA_ANKA_REGISTRY="http://127.0.0.1:8089"
    export ANKA_REGISTRY_LISTEN_ADDRESS=":8089" 
    ```

#### Logging (macOS)

Logs are written to `/Library/Logs/Veertu/AnkaController` by default:
```shell
/Library/Logs/Veertu/AnkaController/anka-controller.INFO
/Library/Logs/Veertu/AnkaController/anka-controller.WARNING
/Library/Logs/Veertu/AnkaController/anka-controller.ERROR
```
 
> You can modify the destination in the `/usr/local/bin/anka-controllerd` file ([reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}})).

You can also watch the logs live (similar to tail -f):
```shell
sudo anka-controller logs
```
 
> The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}}))

### Step 3: Link the Anka CLI Node to the Controller (macOS)

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

> Perform the following steps on the Node where you created your first VM Template.

#### Add the Registry (macOS)

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

#### Push the VM to the Registry (macOS)

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

#### Join to the Controller & Registry (macOS)

{{< include file="_partials/anka-virtualization-cli/getting-started/_join-to-cluster.md" >}}

### Step 4: Start a VM instance using the Controller UI (macOS)

{{< include file="_partials/anka-virtualization-cli/getting-started/_start-vm-instance-using-controller-ui.md" >}}

### Standalone Registry (macOS)

Often we find that customers wish to run the Anka Build Cloud Registry alongside their Anka Nodes to optimize the speed of pulling VM Templates, but keep the Controller hosted in a different location.

In order to run the standalone registry on macOS you'll download and install the [the registry standalone pkg](https://veertu.com/downloads/ankaregistry-mac-latest). Below you will find instructions for configuration.

#### Orientation (macOS/registry)

- Command: `anka-registry [start|stop|restart|status|logs]`
- Default Ports: `8089`
- Binaries and scripts:
    - Registry binary: `/Library/Application Support/Veertu/Anka/bin/anka-registryd`  
    - Launchd daemon: `/Library/LaunchDaemons/com.veertu.anka.registry.plist`
    - Configuration file: `/usr/local/bin/anka-registryd`
- By default, Registry storage is set to: `/Library/Application Support/Veertu/Anka/registry`

#### Configuration (macOS/registry)

1. `sudo anka-registry stop`
1. Edit `/usr/local/bin/anka-registryd` with the ENVs for what you want to change.
1. `sudo anka-registry start`

---

## What next?

- [Prepare and join your machines to the Build Cloud]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "plugins-and-integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "anka-build-cloud/working-with-controller-and-api.md#rest-api">}}).  
