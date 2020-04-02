
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

{{< include file="shared/content/en/docs/Getting Started/partials/_what-we-will-do.md" >}}

{{< include file="shared/content/en/docs/Getting Started/partials/_step1.md" >}}

## Step 2. Install Anka Controller & Registry

> ***NOTE***  
> Perform the following steps on the machine intended to run the Controller & Registry.

### Download the Controller & Registry archive

Download the file called "Cloud Controller & Registry (Run on Linux Instance)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page" >}}.

If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -L -o /tmp/anka-controller-registry.tar.gz https://veertu.com/downloads/ankacontroller-registry-docker-latest
```

#### Untar the archive

```shell
mkdir -p /tmp/anka-controller-registry
cd /tmp/anka-controller-registry
tar -xzvf /tmp/anka-controller-registry.tar.gz
```

#### Configuration

We'll need to do two things:
1. Set the  **external registry address** -- This address is passed to the Nodes so they can download VM Templates.  
2. Mount a volume for the Registry data -- The directory will contain VM disk images and configuration files.

First, edit the `docker-compose.yml`.
1. Under `anka-controller > environment`, find the variable **ANKA_REGISTRY_ADDR**.  
2. Next to it, replace ***\*\*\*EDIT_ME\*\*\**** with the URL of your Registry:
    ```shell
    ANKA_REGISTRY_ADDR: http://<ip>:8089
    ```  
    
{{< highlight shell "hl_lines=21-22" >}}

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
      # This address will be used by the controller. 

  . . .

{{</ highlight >}}

3. Under `anka-registry > volumes`, find the line that says ***# - \*\*\*\*EDIT_ME\*\*\*\*:/mnt/vol***.
4. First, uncomment this line by removing the \# sign from the head of the line. Then replace \*\*\*\*EDIT_ME\*\*\*\* with the path on your machine where you want the Registry files to be saved: 
    ```shell
    - /var/anka:/mnt/vol
    ``` 

{{< highlight shell "hl_lines=17" >}}

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

> ***NOTE***  
> If you're running these containers on mac, you need to change etcd's local volume destination from `/var/etcd-data` to a writable location on your mac.

#### Start the containers

- Ensure you're in the same directory as the `docker-compose.yml`.

```shell
docker-compose up -d
```

`docker-compose` will now build your containers and run the services defined as a daemon.

> ***NOTE***  
> To stop the docker containers, run: `docker-compose down`

### Verify the containers are running
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

#### Logging
Containers are writing logs to stderr, making them available to docker.  
To see the Controller's logs:  
```shell
docker logs --tail 100 -f test_anka-controller_1
```

To see the Registry's logs:  
```shell
docker logs --tail 100 -f test_anka-registry_1
```

The log level can be modified from the default 0 value. The higher the number, the more verbose the logging ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference/#logging)).


{{< include file="shared/content/en/docs/Getting Started/partials/_step3-and-4.md" >}}
