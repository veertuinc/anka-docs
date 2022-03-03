---
title: "Setting up on Linux using Docker"
linkTitle: "Setting up on Linux using Docker"
weight: 1
description: >
  Set up your Anka Build Cloud on Linux using Docker
---

Welcome! This tutorial guides you through setting up your Anka Build Cloud on Linux using Docker and Docker-Compose.

{{< hint warning >}}
**This guide is for first time users and shouldn't be used for upgrading. Please use [the upgrading guide]({{< relref "arm/Anka Build Cloud/upgrading.md" >}}) instead.**
{{< /hint >}}

## Necessary Hardware

1. A machine running Linux to install the Anka Controller & Registry.
2. A Mac to install Anka CLI as a Node.

{{< hint info >}}
While it's possible to run Docker on mac, it's not recommended. An Anka Controller & Registry package exists for native macOS if absolutely necessary.
{{< /hint >}}

## Necessary Software

1. {{< ext-link href="https://docs.docker.com/install/" text="Docker" >}}

2. {{< ext-link href="https://docs.docker.com/compose/install/" text="Docker-Compose" >}} -- Be sure to follow the {{< ext-link href="https://docs.docker.com/install/linux/linux-postinstall/" text="Post Installation setup" >}} in order to run docker-compose without using sudo.  

{{< include file="_partials/intel/Getting Started/_what-we-are-doing.md" >}}

## Step 1: Get familiar with Anka Virtualization

[Please go over our Getting Started guide before proceeding.]({{< relref "arm/Getting Started/creating-your-first-vm.md" >}})

## Step 2: Install the Anka Build Cloud Controller & Registry

{{< hint warning >}}
Perform the following steps on the machine intended to run the Controller & Registry and not the node running the Anka Virtualization software.
{{< /hint >}}

### Download and extract the Controller & Registry

```shell
FULL_FILE_NAME=$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-docker-latest) | cut -d/ -f5)
PARTIAL_FILE_NAME=$(echo $FULL_FILE_NAME | awk -F'.tar.gz' '{print $1}')
mkdir -p $PARTIAL_FILE_NAME
cd $PARTIAL_FILE_NAME
curl -Ls https://veertu.com/downloads/ankacontroller-registry-docker-latest -o $FULL_FILE_NAME
tar -xzvf $FULL_FILE_NAME
```

You can also manually download the file called "Cloud Controller & Registry (Run on Linux Instance)" from the {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}

#### Configuration

We'll need to do two things:

1. Set the  **external registry address** -- This address is passed to the anka nodes so they can pull VM Templates from the Registry.

2. Mount a volume for the Registry data -- The directory containing VM Template/Tag layers/files and configuration metadata.

First, edit the `controller/controller.env`:

1. Find the variable **ANKA_ANKA_REGISTRY** and set it to the proper URL (remove the comment). It should look like:

    ```shell
    ANKA_ANKA_REGISTRY: http://<ip/fqdn>:8089
    ```

Next, edit the `docker-compose.yml` (in the package root, not under the `registry` directory):

1. Under `anka-registry > volumes`, find the line that says ***# - \*\*\*\*EDIT_ME\*\*\*\*:/mnt/vol***. Change this to include the host directory you wish to mount into the container and which will be used to store the data. It should look like:

    ```shell
    - /var/registry:/mnt/vol
    ```

{{< hint info >}}
If you're running these containers on mac (which you should avoid), you need to also change volume source from `/var/etcd-data` and `/var/registry` to a writable location on your mac.
{{< /hint >}}

#### Start the containers

In the root package directory, execute:

```shell
docker-compose up -d
```

### Verify the containers are running

```shell
docker ps -a

CONTAINER ID        IMAGE                 COMMAND                  CREATED              STATUS              PORTS                    NAMES
aa1de7c150e7        test_anka-controller   "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp       test_anka-controller_1
0ac3a6f8b0a1        test_anka-registry     "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:8089->8089/tcp   test_anka-registry_1
03787d28d3a3        test_etcd              "/usr/bin/etcd --dat…"   About a minute ago   Up About a minute                            test_etcd_1
```

{{< include file="_partials/intel/Getting Started/_controller-listening-on-80-and-orientation.md" >}}

#### Configuration and scripts

Any non-default configuration changes are done by editing the .env files, or directly in `docker-compose.yml`.

[A full configuration reference is available.]({{< relref "arm/Anka Build Cloud/configuration-reference.md" >}})

#### Logging

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

The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference]({{< relref "arm/Anka Build Cloud/configuration-reference.md#logging" >}}))
{{< /hint >}}

## Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

{{< hint info >}}
Perform the following steps on the host machine/node where you created your first VM.
{{< /hint >}}

### Add the Registry

{{< include file="_partials/intel/Getting Started/_add-the-registry.md" >}}

### Push the VM to the Registry

{{< include file="_partials/intel/Getting Started/_push-vm-to-registry.md" >}}

### Join to the Controller & Registry

In order for the host/node to perform controller tasks (pull, start, delete, etc), it must be joined to the Anka Build Cloud. You can do this by executing:

{{< include file="_partials/intel/Getting Started/_join-to-cluster.md" >}}

## Step 4: Start a VM instance using the Controller UI

{{< include file="_partials/intel/Getting Started/_start-vm-instance-using-controller-ui.md" >}}

## Step 5: Orientate to your new environment

### Anka Controller Container

- Default Ports: 80
- Binary in the container: `/bin/anka-controller`
- Configuration files: Configuration is done through docker-compose file, the `controller/controller.env`, or through environment variables.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.  
- Data Storage: No data is saved on disk.

### Anka Registry Container

- Default Ports: 8089
- Binary in the container: `/bin/anka-registry`
- Configuration files: Configuration is done through docker-compose file, the `registry/registry.env`, or through environment variables.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.  
- Data will be written to: No default; You must set it in docker-compose.yml.

### ETCD Container

- Default Ports: N/A
- Binary in the container: `/usr/bin/etcd`
- Configuration files: Configuration is done through docker-compose file, the `etcd/etcd.env`, or through environment variables.
- Logs will be written to: STDOUT/ERR. It's possible to get the logs through `docker logs` command.  
- Data will be written to: `/var/etcd-data`

---

{{< include file="_partials/Anka Build Cloud/etcd-snapshotting.md" >}}

---

## What next?

- [Prepare and join your machines to the Build Cloud]({{< relref "arm/Anka Build Cloud/preparing-and-joining-your-nodes.md" >}}).  
- Browse the [Anka CLI Command Reference]({{< relref "arm/Anka Virtualization/command-reference.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "arm/CI Plugins and Integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "arm/Anka Build Cloud/working-with-controller-and-api.md#rest-api">}}).  
