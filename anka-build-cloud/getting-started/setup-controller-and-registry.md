---
title: "Setting up the Controller & Registry"
weight: 1
---

Welcome! This tutorial guides you through setting up your Anka Build Cloud Controller & Registry on either Linux/Docker or MacOS.

{{< hint warning >}}
**This guide is for first time users and shouldn't be used for upgrading. Please use [the upgrading guide]({{< relref "anka-build-cloud/upgrading.md" >}}) instead.**
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

{{< include file="_partials/anka-build-cloud/_what-we-are-doing.md" >}}

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

{{< include file="_partials/anka-build-cloud/_controller-listening-on-80-and-orientation.md" >}}

##### Configuration and scripts

Any non-default configuration changes are done by editing the .env files, or directly in `docker-compose.yml`.

[A full configuration reference is available.]({{< relref "anka-build-cloud/configuration-reference.md" >}})

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

The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}}))
{{< /hint >}}

### Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

{{< hint info >}}
Perform the following steps on the host machine/node where you created your first VM.
{{< /hint >}}

#### Add the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

#### Push the VM to the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

#### Join to the Controller & Registry

In order for the host/node to perform controller tasks (pull, start, delete, etc), it must be joined to the Anka Build Cloud. You can do this by executing:

{{< include file="_partials/anka-virtualization-cli/getting-started/_join-to-cluster.md" >}}

### Step 4: Start a VM instance using the Controller UI

{{< include file="_partials/anka-virtualization-cli/getting-started/_start-vm-instance-using-controller-ui.md" >}}

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

### ETCD Container

ETCD is a critical piece of your Anka Build Cloud. It stores tasks, Node and VM Instance information, and many other types of state for the Controller service. If it's not functional, the Controller will throw failures. We'll orient ourself to the basics and defaults of the ETCD service we include with our package and then describe how to maintain it for optimal performance and stability.

- Default Ports: 2379
- Configuration files: Configuration is done through the docker-compose file (as ENV variables under `environment:`), or the `etcd/etcd.env` file.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.
- The data directory and the "DB SIZE" you see under the `endpoint status` are not the same thing. Disk usage can be significantly different, so you should monitor both independently.

#### Testing ETCD Performance

Testing ETCD performance to ensure it will run properly on your chosen hardware. This can be done by running `etcdctl check perf` command inside of the docker container:

```bash
❯ docker exec -it anka.etcd bash -c "etcdctl --endpoints=http://localhost:2379 check perf"
 60 / 60 Boooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo! 100.00% 1m0s
PASS: Throughput is 150 writes/s
PASS: Slowest request took 0.002838s
PASS: Stddev is 0.000141s
PASS
```

#### Compaction and Defragmentation

In versions of the Anka Build Cloud <= 1.27.0, we would have the Controller trigger a defragmentation every 3 hours. This is no longer the case and **we perform no defragmentation by default**. It actually turns out that defragmentation is not fully necessary (and dangerous since it prevents writing to etcd, even when etcd is clustered).

Any previously used etcd key/values are re-used when they're no longer needed (see https://etcd.io/docs/v3.5/op-guide/maintenance/#defragmentation):

> After compacting the keyspace, the backend database may exhibit internal fragmentation. Any internal fragmentation is space that is free to use by the backend but still consumes storage space. Compacting old revisions internally fragments etcd by leaving gaps in backend database. Fragmented space is available for use by etcd but unavailable to the host filesystem. In other words, deleting application data does not reclaim the space on disk.

History Compaction seems to be the most important part for keeping DB size from growing uncontrollably. You will want to not limit ETCD initially until you have a fully used production environment, else you won't know how large the DB size can grow before it stabilizes.

We recommend graphing and monitoring the following etcd metrics:

- `etcd_mvcc_db_total_size_in_bytes`: Shows the database size including free space waiting for defragmentation.
- `etcd_mvcc_db_total_size_in_use_in_bytes`: Indicates the actual database usage after a history compaction.

Again, the `etcd_mvcc_db_total_size_in_bytes` increases only when the former is close to it, meaning when both of these metrics are close to the quota, a history compaction is required to avoid triggering the [space quota](https://etcd.io/docs/v3.5/op-guide/maintenance/#space-quota). Defragmentation is only needed when the in_use remains well below the total metric for a period of time (ensure it does not happen while the Anka Cloud is being actively used).

##### History Compaction

By default, we set and recommend `30m` compaction. There are dangers in having this happen too soon or too late, but they are entirely dependent on your environment size and usage. You can read more about this [on the official documentation](https://etcd.io/docs/v3.5/op-guide/maintenance/#history-compaction-v3-api-key-value-database).

##### Defragmentation

In versions of the Anka Build Cloud <= 1.27.0, we would have the Controller trigger a defragmentation every 3 hours. We've disabled this by default and administrators should be aware that the DB size will eventually stop increasing once the Anka environment is fully utilized at least once. Defragmentation can be executed manually, but it's not necessary and you **must ensure it does not happen while the Anka Cloud is being actively used.** [Read about ETCD defragmentation here.](https://etcd.io/docs/v3.5/op-guide/maintenance/#defragmentation)

{{< include file="_partials/anka-build-cloud/etcd-snapshotting.md" >}}

---

We recommend looking through the [entire ETCD operations guide](https://etcd.io/docs/v3.5/op-guide/) to gain a better understanding of everything we've spoken about here, and more!

## Standalone Registry (Linux)

Often we find that customers wish to only run the Anka Build Cloud Registry and not the Controller. This is a useful setup when you're using [a Controller-less Build Cloud]({{< relref "plugins-and-integrations/_index.md#controller-less-registry-only" >}}).

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

{{< include file="_partials/anka-build-cloud/_what-we-are-doing.md" >}}

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
  curl http://<ip>/api/v1/status
  ```

{{< include file="_partials/anka-virtualization-cli/getting-started/_controller-listening-on-80-and-orientation.md" >}}

##### Configuration and scripts ([reference]({{< relref "anka-build-cloud/configuration-reference.md" >}}))

The Anka Controller **AND Registry** command is installed into `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges:
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
When `sudo anka-controller start` is executed, the script will use `launchd` to load the daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`.
 - The Anka Controller & Registry run script is `/usr/local/bin/anka-controllerd`. This file acts as a run script **and configuration file**. You can modify it to change the default ports used by adding the proper option or ENV. For example, if you want to run the Registry on a different port and use 127.0.0.1, you would add the following above the `"$CONTROLLER_BIN"` line ([reference]({{< relref "anka-build-cloud/configuration-reference.md" >}})): 
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
 
> You can modify the destination in the `/usr/local/bin/anka-controllerd` file ([reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}})).

You can also watch the logs live (similar to tail -f):
```shell
sudo anka-controller logs
```
 
> The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}}))

### Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

> Perform the following steps on the Node where you created your first VM Template.

#### Add the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

#### Push the VM to the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

#### Join to the Controller & Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_join-to-cluster.md" >}}

### Step 4: Start a VM instance using the Controller UI

{{< include file="_partials/anka-virtualization-cli/getting-started/_start-vm-instance-using-controller-ui.md" >}}

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

- [Prepare and join your machines to the Build Cloud]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "plugins-and-integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "anka-build-cloud/working-with-controller-and-api.md#rest-api">}}).  
