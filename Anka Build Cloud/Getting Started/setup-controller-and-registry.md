---
title: "Setting up the Controller & Registry"
weight: 1
---

Welcome! This tutorial guides you through setting up your Anka Build Cloud Controller & Registry on either Linux/Docker or MacOS.

{{< hint warning >}}
**This guide is for first time users and shouldn't be used for upgrading. Please use [the upgrading guide]({{< relref "Anka Build Cloud/upgrading.md" >}}) instead.**
{{< /hint >}}

## Linux / Docker

### Necessary Hardware

1. A machine running Linux to install the Anka Controller & Registry.
2. A Mac to install Anka CLI as a Node.

{{< hint info >}}
While it's possible to run Docker on mac, it's not recommended. An Anka Controller & Registry package exists for native macOS if absolutely necessary.
{{< /hint >}}

### Necessary Software

1. {{< ext-link href="https://docs.docker.com/install/" text="Docker" >}}

2. {{< ext-link href="https://docs.docker.com/compose/install/" text="Docker-Compose" >}} -- Be sure to follow the {{< ext-link href="https://docs.docker.com/install/linux/linux-postinstall/" text="Post Installation setup" >}} in order to run docker-compose without using sudo.  

{{< include file="_partials/Anka Build Cloud/_what-we-are-doing.md" >}}

#### Download and extract the Controller & Registry

```shell
FULL_FILE_NAME=$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-docker-latest) | cut -d/ -f5)
PARTIAL_FILE_NAME=$(echo $FULL_FILE_NAME | awk -F'.tar.gz' '{print $1}')
mkdir -p $PARTIAL_FILE_NAME
cd $PARTIAL_FILE_NAME
curl -Ls https://veertu.com/downloads/ankacontroller-registry-docker-latest -o $FULL_FILE_NAME
tar -xzvf $FULL_FILE_NAME
```

You can also manually download the file called "Cloud Controller & Registry (Run on Linux Instance)" from the {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}

##### Configuration

We'll need to do two things:

1. Set the  **external registry address** -- This address is passed to the anka nodes so they can pull VM Templates from the Registry.

2. Mount a volume for the Registry data -- The directory containing VM Template/Tag layers/files and configuration metadata.

First, edit the `controller/controller.env`:

1. Find the variable **ANKA_ANKA_REGISTRY** and set it to the proper URL (remove the comment). It should look like:

    ```shell
    ANKA_ANKA_REGISTRY="http://<ip/fqdn>:8089"
    ```

Next, edit the `docker-compose.yml` (in the package root, not under the `registry` directory):

1. Under `anka-registry > volumes`, find the line that says ***# - \*\*\*\*EDIT_ME\*\*\*\*:/mnt/vol***. Change this to include the host directory you wish to mount into the container and which will be used to store the data. It should look like:

    ```shell
    - /var/registry:/mnt/vol
    ```

{{< hint info >}}
If you're running these containers on mac (which you should avoid), you need to also change volume source from `/var/etcd-data` and `/var/registry` to a writable location on your mac.
{{< /hint >}}

##### Start the containers

In the root package directory, execute:

```shell
docker-compose up -d
```

#### Verify the containers are running

```shell
docker ps -a

CONTAINER ID        IMAGE                 COMMAND                  CREATED              STATUS              PORTS                    NAMES
aa1de7c150e7        test_anka-controller   "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp       test_anka-controller_1
0ac3a6f8b0a1        test_anka-registry     "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:8089->8089/tcp   test_anka-registry_1
03787d28d3a3        test_etcd              "/usr/bin/etcd --dat…"   About a minute ago   Up About a minute                            test_etcd_1
```

{{< include file="_partials/Anka Build Cloud/_controller-listening-on-80-and-orientation.md" >}}

##### Configuration and scripts

Any non-default configuration changes are done by editing the .env files, or directly in `docker-compose.yml`.

[A full configuration reference is available.]({{< relref "Anka Build Cloud/configuration-reference.md" >}})

##### Logging

Containers are writing logs to STDOUT+ERR, making them available to Docker.  

To see the Controller's logs:

```shell
docker logs --tail 100 -f test_anka-controller_1
```

To see the Registry's logs:

```shell
docker logs --tail 100 -f test_anka-registry_1
```

[By default, docker does not do log-rotation. As a result, log-files stored by the default json-file logging driver logging driver can cause a significant amount of disk space to be used for containers that generate much output, which can lead to disk space exhaustion.](https://docs.docker.com/config/containers/logging/configure/)

{{< hint info >}}
##### Troubleshooting tip

The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "Anka Build Cloud/configuration-reference.md#logging" >}}))
{{< /hint >}}

### Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

{{< hint info >}}
Perform the following steps on the host machine/node where you created your first VM.
{{< /hint >}}

#### Add the Registry

{{< include file="_partials/intel/Getting Started/_add-the-registry.md" >}}

#### Push the VM to the Registry

{{< include file="_partials/intel/Getting Started/_push-vm-to-registry.md" >}}

#### Join to the Controller & Registry

In order for the host/node to perform controller tasks (pull, start, delete, etc), it must be joined to the Anka Build Cloud. You can do this by executing:

{{< include file="_partials/intel/Getting Started/_join-to-cluster.md" >}}

### Step 4: Start a VM instance using the Controller UI

{{< include file="_partials/intel/Getting Started/_start-vm-instance-using-controller-ui.md" >}}

### Step 5: Orientate to your new environment

#### Anka Controller Container

- Default Ports: 80
- Binary in the container: `/bin/anka-controller`
- Configuration files: Configuration is done through docker-compose file, the `controller/controller.env`, or through environment variables.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.  
- Data Storage: No data is saved on disk.

#### Anka Registry Container

- Default Ports: 8089
- Binary in the container: `/bin/anka-registry`
- Configuration files: Configuration is done through docker-compose file, the `registry/registry.env`, or through environment variables.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.  
- Data will be written to: No default; You must set it in docker-compose.yml.

#### ETCD Container

- Default Ports: N/A
- Binary in the container: `/usr/bin/etcd`
- Configuration files: Configuration is done through docker-compose file, the `etcd/etcd.env`, or through environment variables.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.  
- Data will be written to: `/var/etcd-data`

{{< include file="_partials/Anka Build Cloud/etcd-snapshotting.md" >}}

## Standalone Registry (Linux)

Often we find that customers wish to only run the Anka Build Cloud Registry and not the Controller. This is a useful setup when you're using [a Controller-less Build Cloud]({{< relref "Plugins and Integrations/_index.md#controller-less-build-cloud-registry-only" >}}).

In order to run the standalone registry you'll:

1. Follow [Step #1]({{< relref "#step-2-install-the-anka-build-cloud-controller--registry" >}}) above, but:
  - skip the sections to configure the controller. This means only set the registry volume.
  - before you `docker-compose up -d`, comment out or remove the Controller and ETCD services from the yml.

---

## MacOS

{{< hint info >}}
The macOS package will install and run using Apple's Rosetta. There is no native arm package at this time.
{{< /hint >}}

### Necessary Hardware

1. A Mac to install the Anka Controller & Registry.
2. A second Mac to install the Anka CLI (the "Node").

> You can complete this tutorial with only one machine running Mac OS, but it's not recommended.

{{< include file="_partials/Anka Build Cloud/_what-we-are-doing.md" >}}

#### Download the Controller & Registry PKG

Download the file called "Cloud Controller & Registry (Run on Mac)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}
If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-mac-latest) | cut -d/ -f5) https://veertu.com/downloads/ankacontroller-registry-mac-latest
```

#### Install the Controller & Registry PKG

Double click on the .pkg to start the UI install process.
- Or, you can install the package using the command line:
    ```shell
    sudo installer -pkg ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-mac-latest) | cut -d/ -f5) -target /
    ```

#### Verify your installation
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

##### Configuration and scripts ([reference]({{< relref "Anka Build Cloud/configuration-reference.md" >}}))

The Anka Controller **AND Registry** command is installed into `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges:
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
When `sudo anka-controller start` is executed, the script will use `launchd` to load the daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`.
 - The Anka Controller & Registry run script is `/usr/local/bin/anka-controllerd`. This file acts as a run script **and configuration file**. You can modify it to change the default ports used by adding the proper option or ENV. For example, if you want to run the Registry on a different port and use 127.0.0.1, you would add the following above the `"$CONTROLLER_BIN"` line ([reference]({{< relref "Anka Build Cloud/configuration-reference.md" >}})): 
    ```shell
    export ANKA_ANKA_REGISTRY="http://127.0.0.1:8081"
    export ANKA_REGISTRY_LISTEN_ADDRESS=":8081" 
    ```

##### Logging

Logs are written to `/Library/Logs/Veertu/AnkaController` by default:
```shell
/Library/Logs/Veertu/AnkaController/anka-controller.INFO
/Library/Logs/Veertu/AnkaController/anka-controller.WARNING
/Library/Logs/Veertu/AnkaController/anka-controller.ERROR
```
 
> You can modify the destination in the `/usr/local/bin/anka-controllerd` file ([reference]({{< relref "Anka Build Cloud/configuration-reference.md#logging" >}})).

You can also watch the logs live (similar to tail -f):
```shell
sudo anka-controller logs
```
 
> The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "Anka Build Cloud/configuration-reference.md#logging" >}}))

### Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

> Perform the following steps on the Node where you created your first VM Template.

#### Add the Registry

{{< include file="_partials/intel/Getting Started/_add-the-registry.md" >}}

#### Push the VM to the Registry

{{< include file="_partials/intel/Getting Started/_push-vm-to-registry.md" >}}

#### Join to the Controller & Registry

{{< include file="_partials/intel/Getting Started/_join-to-cluster.md" >}}

### Step 4: Start a VM instance using the Controller UI

{{< include file="_partials/intel/Getting Started/_start-vm-instance-using-controller-ui.md" >}}

### Step 5: Orientate to your new environment

#### Anka Controller and Registry package

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

#### Standalone Anka Registry

- Default Ports: 80
- Binaries and scripts:
    Anka Registry for Mac binary is installed at: `/Library/Application Support/Veertu/Anka/bin/ankaregd`  
    Launchd daemon: `/Library/LaunchDaemons/com.veertu.anka.registry.plist`
- Configuration files: Configuration for this package is done by altering the Launchd daemon xml file at `/Library/LaunchDaemons/com.veertu.anka.registry.plist`. Be sure to unload it first.
- Data will be saved in: `/Library/Application Support/Veertu/Anka/registry`

---

## What next?

- [Prepare and join your machines to the Build Cloud]({{< relref "Anka Build Cloud/Getting Started/preparing-and-joining-your-nodes.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "Plugins and Integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#rest-api">}}).  
