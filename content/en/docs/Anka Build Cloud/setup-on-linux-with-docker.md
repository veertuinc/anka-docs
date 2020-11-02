---
title: "Setting up on Linux using Docker"
linkTitle: "Setting up on Linux using Docker"
weight: 1
description: >
  Set up your Anka Build Cloud on Linux using Docker
---

Welcome! This tutorial guides you through setting up your Anka Build Cloud on Linux using Docker and Docker-Compose.

## Necessary Hardware 

1. A machine running Linux to install the Anka Controller & Registry.
2. A Mac to install Anka CLI as a Node.

> While it's possible to run Docker on mac, it's not recommended. An Anka Controller & Registry package exists.

## Necessary Software
1. {{< ext-link href="https://docs.docker.com/install/" text="Docker" >}} 
2. {{< ext-link href="https://docs.docker.com/compose/install/" text="Docker-Compose" >}} -- Be sure to follow the {{< ext-link href="https://docs.docker.com/install/linux/linux-postinstall/" text="Post Installation setup" >}} in order to run docker-compose without using sudo.  

{{< include file="shared/content/en/docs/Getting Started/partials/_what-we-are-doing.md" >}}

## Step 1: Get familiar with Anka Virtualization

### Install the Anka Virtualization package

#### Download and install with your terminal

{{< include file="shared/content/en/docs/Anka Virtualization/partials/_download-and-install-with-terminal.md" >}}

#### Verify the installation

{{< include file="shared/content/en/docs/Anka Virtualization/partials/_verify-installation.md" >}}

For Anka CLI commands and options, see the [Command Reference]({{< relref "docs/Anka Virtualization/command-reference.md" >}}).

## Obtain the macOS installer

{{< include file="shared/content/en/docs/Getting Started/partials/_obtain-macos-installer.md" >}}

## Create the VM Template

### Using the Anka UI

### Using the Anka CLI

{{< include file="shared/content/en/docs/Anka Virtualization/partials/create/_example.md" >}}

> You can find detailed instructions for [`anka create`]({{< relref "docs/Anka Virtualization/command-reference.md#create" >}}) [here.]({{< relref "docs/Getting Started/creating-your-first-vm.md" >}})

> You can continue on to Step 2 while you wait for this to finish.

## Step 2: Install the Anka Build Cloud Controller & Registry

> Perform the following steps on the machine intended to run the Controller & Registry.

### Download and extract the Controller & Registry

```shell
FULL_FILE_NAME=$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/ankacontroller-registry-docker-latest) | cut -d/ -f4)
PARTIAL_FILE_NAME=$(echo $FULL_FILE_NAME | awk -F'.tar.gz' '{print $1}')
mkdir -p $PARTIAL_FILE_NAME
cd $PARTIAL_FILE_NAME
curl -Ls https://veertu.com/downloads/ankacontroller-registry-docker-latest -o $FULL_FILE_NAME
tar -xzvf $FULL_FILE_NAME
```

You can also manually download the file called "Cloud Controller & Registry (Run on Linux Instance)" from the {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}

#### Configuration

We'll need to do two things:
1. Set the  **external registry address** -- This address is passed to the Nodes so they can pull VM Templates from the Registry.  
2. Mount a volume for the Registry data -- The directory containing VM disk images and configuration files.

First, edit the `docker-compose.yml`.
1. Under `anka-controller > environment`, find the variable **ANKA_REGISTRY_ADDR**.  
2. Next to it, replace ***\*\*\*EDIT_ME\*\*\**** with the URL of your Registry: 
    
    {{< highlight shell "hl_lines=22" >}}

      . . .

      anka-controller:
        build:
          context: .
          dockerfile: anka-controller.docker
        ports:
          - "80:80"
        # To change the port, change the above line: - "CUSTOM_PORT:80"
        ######   EDIT HERE FOR TLS  ########
        # volumes:
          # Path to ssl certificates directory 
          # - ****EDIT_ME****:/mnt/cert
        depends_on:
          - etcd
          #  - beanstalk
          - anka-registry
        restart: always
        environment:

          # Address of anka registry. this address will be passed to your build nodes
          ANKA_REGISTRY_ADDR: ****EDIT_ME****               
          
          # Local Anka registry address
          # This address is used by the Controller. 

      . . .

    {{</ highlight >}}

    It should look like:
    ```shell
    ANKA_REGISTRY_ADDR: http://<ip>:8089
    ``` 

3. Under `anka-registry > volumes`, find the line that says ***# - \*\*\*\*EDIT_ME\*\*\*\*:/mnt/vol***.
4. First, uncomment this line by removing the \# sign from the head of the line. Then replace \*\*\*\*EDIT_ME\*\*\*\* with the path on your machine where you want the Registry files to be saved: 

    {{< highlight shell "hl_lines=15" >}}

      . . .

      anka-registry:
      build:
          context: .
          dockerfile: anka-registry.docker
      ports:
          - "8089:8089"
      # To change the port change the above line: - "CUSTOM_PORT:8089"
      restart: always
      volumes:
          ######   EDIT HERE  ########
          # Path to registry data folder.
          # VM data files and logs will be saved in this folder
          # - ****EDIT_ME****:/mnt/vol

          # Path to ssl certificates directory 
          # - ****EDIT_ME****:/mnt/cert

      . . .

    {{< /highlight >}}

    It should look like:

    ```shell
    - /var/anka:/mnt/vol
    ``` 


> If you're running these containers on mac, you need to also change etcd's local volume destination from `/var/etcd-data` to a writable location on your mac.

#### Start the containers

> Ensure you're in the same directory as the `docker-compose.yml`.

```shell
docker-compose up -d
```

This command builds your containers and runs the services defined as a daemon.

> To stop the docker containers, run: `docker-compose down`

### Verify the containers are running
```shell
docker ps

CONTAINER ID        IMAGE                 COMMAND                  CREATED              STATUS              PORTS                    NAMES
aa1de7c150e7        test_anka-controller   "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp       test_anka-controller_1
0ac3a6f8b0a1        test_anka-registry     "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:8089->8089/tcp   test_anka-registry_1
03787d28d3a3        test_etcd              "/usr/bin/etcd --dat…"   About a minute ago   Up About a minute                            test_etcd_1
```

{{< include file="shared/content/en/docs/Getting Started/partials/_controller-listening-on-80-and-orientation.md" >}}

#### Configuration and scripts
Make configurations by editing `docker-compose.yml`. Most of the stuff you can configure is listed there but commented out.

Check out the variables under the `environment` section of each service. [A full reference is available.](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference)

#### Logging
Containers are writing logs to stderr, making them available to Docker.  
To see the Controller's logs:  
```shell
docker logs --tail 100 -f test_anka-controller_1
```

To see the Registry's logs:  
```shell
docker logs --tail 100 -f test_anka-registry_1
```

> The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference/#logging))

## Step 3: Link the Anka CLI Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

> Perform the following steps on the Node where you created your first VM Template.

### Add the Registry

{{< include file="shared/content/en/docs/Getting Started/partials/_add-the-registry.md" >}}

### Push the VM to the Registry

{{< include file="shared/content/en/docs/Getting Started/partials/_push-vm-to-registry.md" >}}

### Join to the Controller & Registry

{{< include file="shared/content/en/docs/Getting Started/partials/_join-to-cluster.md" >}}

## Step 4: Start a VM instance using the Controller UI

{{< include file="shared/content/en/docs/Getting Started/partials/_start-vm-instance-using-controller-ui.md" >}}

## Orientation

### Anka Controller Docker container
- Default Ports: 80
- Binaries and scripts: `/usr/bin/anka-controller`
- Configuration files: Configuration is done through docker-compose file or through environment variables.
- Logs will be written to: `/var/log/anka-controller`. It's also possible to get the logs through `docker logs` command.   
- Data Storage: No data is saved on the container.

### Anka Registry Docker container
- Default Ports: 8089
- Binaries and scripts: `/usr/bin/anka-registry`
- Configuration files: Configuration is done through docker-compose file or through environment variables.
- Logs will be written to: `/var/log/anka-registry`. It's also possible to get the logs through `docker logs` command.  
- Registry data will be written to: `/mnt/vol`

## What next?

- Browse the [Anka CLI Command Reference]({{< relref "docs/Anka Virtualization/command-reference.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "docs/CI Plugins and Integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "docs/Anka Build Cloud/working-with-controller-and-api.md#api">}}).  
- Learn how to work with [USB devices]({{< relref "docs/Anka Virtualization/working-with-usb-devices.md">}})