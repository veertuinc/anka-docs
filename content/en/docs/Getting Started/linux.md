
---
title: "Getting started with Anka Build Cloud on Linux Using Docker"
linkTitle: "Run Anka Build Cloud on Linux Using Docker"
weight: 3
description: >
  Set up your Anka Build Cloud on Linux.
---

Welcome! this tutorial will guide you through setting up your Anka Build Cloud on Linux using Docker and docker-compose.


## Necessary hardware 

1. A machine running Linux to install Anka Controller + Registry on. 
2. At least one Mac running Mac OS to install Anka CLI as a Node.

> ***note***  
> While it's possible to run docker on mac, it's not recommended.  
> Performance should be far from optimal.

## Necessary software
We will be using on your Linux machine, so if you don't have docker and docker-compose installed please install it now.
{{< ext-link href="https://docs.docker.com/install/" text="Install docker" >}}.  
{{< ext-link href="https://docs.docker.com/compose/install/" text="Install docker-compose" >}}.  
Be sure to follow the {{< ext-link href="https://docs.docker.com/install/linux/linux-postinstall/" text="Post Installation setup" >}} in order to run docker-compose without using sudo.  


## What we will do in this tutorial
1. Create your first VM
2. Install Anka Controller and Registry
3. Add one or more Macs as Nodes
4. Start a VM instance using the Anka Build Cloud web interface

## Step 1. Create your first VM


**Perform the following steps on a mac machine that is intended to be a Node.**


{{< include file="shared/content/en/docs/Getting Started/first-vm.md" >}}
For more commands and options, see [Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).
While you wait for the VM creation, continue and perform step 2.

## Step 2. Install Anka Controller and Registry

**Perform the following steps on the machine intended to run the Controller and Registry**

### Download the tar package
Download the file called "Cloud Controller & Registry (Run on Linux Instance)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page" >}}.
If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -L -o anka-controller-registry-1.5.2-ce0d3271.tar.gz https://veertu.com/downloads/ankacontroller-registry-docker-latest
```
The file name is constructed like this - `anka-controller-registry--VERSION-HASH.tar.gz`. We follow semantic versioning so the version number is divided to `MAJOR`, `MINOR` and `PATCH`. 

### Install
Go into the directory where you downloaded the tar and untar it.
```shell
tar -xzvf anka-controller-registry-1.5.2-ce0d3271.tar.gz
```

We'll need to configure two things, first one is the **external registry address**. This address is passed to the build nodes, so they can download VM templates.  
The second thing, is to mount a volume for the Registry data. The directory will contain VM disk images and configuration files.

#### Configure 
Edit the file `docker-compose.yml`.  
Under `anka-controller > environment`, find the variable **ANKA_REGISTRY_ADDR** (highlighted in the example below).  
Replace ***\*\*\*EDIT_ME\*\*\**** with the URL of your Registry.  
> Example:  
> **ANKA_REGISTRY_ADDR: http://192.168.1.10:8089**  
> In this example the URL http://192.168.1.10:8089 should be accessible from your mac machine

{{< highlight shell "hl_lines=21-22" >}}
  snip...

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
      # This address will be used by the controller. 


  snip...

{{< /highlight >}}

Under `anka-registry > volumes`, find the line that says ***# - \*\*\*\*EDIT_ME\*\*\*\*:/mnt/vol*** (highlighted in the example below). First, uncomment this line by removing the \# sign from the head of the line.  
Then replace \*\*\*\*EDIT_ME\*\*\*\* with the path on your machine where you want the Registry files to be saved.  

> Example:  
> **- /var/anka:/mnt/vol**  
> In this example **/var/anka** is an existing path on the host machine


{{< highlight shell "hl_lines=17" >}}
  snip...



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

  snip...

{{< /highlight >}}

#### Run 
In the same working directory, execute the `docker-compose up` command.  
```shell
docker-compose up -d
```
`docker-compose` will now build your containers and run the services defined as a daemon.

### Verify your installation
Execute the `docker ps` command:
```shell
docker ps
```
You should be seeing a response similar to the following:
```shell
CONTAINER ID        IMAGE                 COMMAND                  CREATED              STATUS              PORTS                    NAMES
aa1de7c150e7        tut_anka-controller   "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp       tut_anka-controller_1
0ac3a6f8b0a1        tut_anka-registry     "/bin/bash -c 'anka-…"   About a minute ago   Up About a minute   0.0.0.0:8089->8089/tcp   tut_anka-registry_1
03787d28d3a3        tut_etcd              "/usr/bin/etcd --dat…"   About a minute ago   Up About a minute                            tut_etcd_1



```
Anka Controller should be listening on port 80 (http). Try pointing your browser to the machine's ip or host name. You can use `localhost` or `127.0.0.1` when using the machine the server is installed on.

Your new dashboard should look like the picture below

![How your new dashboard looks like](/images/getting-started/new-dashboard.png)


### Orientation
#### Running Applications
Let's take a look on what is now running on your machine.
1. Anka Controller serving web UI and REST API on port 80.
2. Anka Registry serving REST API on port 8089.
3. ETCD database server serving on ports 2379 and 2380 internally on the docker network. This server is used internally by the Anka Controller
#### Configuration and scripts
Configuration is done by editing `docker-compose.yml`.  
Most of the stuff you can configure is listed there but commented out.
Check out the variables under the `environment` section of each service.

#### Logs
Containers are writing logs to stderr, making them available to docker.  
To see the Controller's logs (Assuming your container is called `anka-controller-1`):  
```shell
docker logs --follow anka-controller-1
```

To see the Registry's logs  (Assuming your container is called `anka-registry-1`):  
```shell
docker logs --follow anka-registry-1
```

The log level is a number starting with 0 as the lowest, the higher the log level means more verbose. The default log level is 0 . 


Great! now that we have our Anka Controller and Registry up and running let's add Nodes!


## Step 3. Add Nodes

**Perform this on the machine you created your first VM on**


{{< include file="shared/content/en/docs/Getting Started/push-to-registry.md" >}}

After the push is finished you should be able to see your new Template in the "templates" section.

![Your first template](/images/getting-started/push-template.png)


{{< include file="shared/content/en/docs/Getting Started/add-node.md" >}}

**Repeat this process on other machines that you want to join as Nodes (Anka CLI needs to be installed)**


## Step 4. Start a VM instance using the Anka Dashboard


{{< include file="shared/content/en/docs/Getting Started/start-vm-dash.md" >}}



## Where to go next?

Browse [Anka CLI Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).  
Connect your cloud to a [CI server]({{< relref "docs/Anka Build Cloud/CI Plugins/_index.md" >}}).  
Find out how to use the [Controller REST API]({{< relref "docs/Anka Build Cloud/controller-api.md">}}).  
Learn how to work with [USB devices]({{< relref "docs/Anka Build Cloud/using-real-devices-attached-to-anka-vms.md">}})

