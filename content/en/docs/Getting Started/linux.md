
---
title: "Getting started with Anka Build Cloud on Linux using Docker"
linkTitle: "Run Anka Build Cloud on Linux using Docker"
weight: 3
description: >
  Set up your Anka Build Cloud on Linux.
---

Welcome! This tutorial will guide you through setting up your Anka Build Cloud on Linux using Docker and docker-compose.

## Necessary hardware 

1. A machine running Linux to install the Anka Controller & Registry.
2. A Mac to install Anka CLI as a Node.

> ***NOTE***  
> While it's possible to run docker on mac, it's not recommended. An Anka Controller & Registry package exists.

## Necessary software
1. Docker and Docker Compose
    - {{< ext-link href="https://docs.docker.com/install/" text="Install docker" >}}.  
    - {{< ext-link href="https://docs.docker.com/compose/install/" text="Install docker-compose" >}}. Be sure to follow the {{< ext-link href="https://docs.docker.com/install/linux/linux-postinstall/" text="Post Installation setup" >}} in order to run docker-compose without using sudo.  

{{< include file="shared/content/en/docs/Getting Started/partials/_step1.md" >}}

## Step 2. Install Anka Controller & Registry

**Perform the following steps on the machine intended to run the Controller & Registry**

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

{{< include file="shared/content/en/docs/Getting Started/partials/_controller-listening-on-80-and-orientation.md" >}}

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

{{< include file="shared/content/en/docs/Getting Started/partials/_step3-and-4.md" >}}
