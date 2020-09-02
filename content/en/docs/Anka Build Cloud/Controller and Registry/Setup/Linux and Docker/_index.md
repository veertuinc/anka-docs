
---
date: 2019-07-03T22:24:47-05:00
title: "Linux & Docker"
linkTitle: "Linux & Docker"
weight: 1
description: >
 Setting up your Anka Build Cloud Controller & Registry on Linux & Docker
---

## Installing

Instructions can be found under our [Getting Started with Linux & Docker guide]({{< relref "docs/Anka Build Cloud/installing-on-linux.md#step-2-install-the-anka-build-cloud-controller--registry" >}})

## Upgrading

1. `docker-compose down` your previous installation
2. Rename the previous directory: `mv ~/anka-docker-controller-registry ~/anka-docker-controller-registry-old`
3. Download and extract the latest version tar:

    ```bash
    mkdir -p ~/anka-docker-controller-registry
    cd ~/anka-docker-controller-registry
    curl -L -o anka-docker-controller-registry.tar.gz https://veertu.com/downloads/ankacontroller-registry-docker-latest
    tar -xzvf anka-docker-controller-registry.tar.gz
    ```

4. Copy the docker-compose file from the previous version's folder into the new: `cp -rfp ~/anka-docker-controller-registry-old/docker-compose.yml ~/anka-docker-controller-registry/`
5. Start the new Cloud Controller & Registry with `--build`: `cd ~/anka-docker-controller-registry && docker-compose up -d --build`

## Uninstalling

1. Go to the controller/registry directory and execute `docker-compose down`.
2. Delete the controller/registry directory.

## Debugging Issues

Anka controller is a server that usually runs as a daemon and errors are usually written to a log.
Check the output of `docker ps`. The anka controller container status should be something like this.

```bash
“Up for x amount of time”. For example:
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                    NAMES
ae6b522fdec4        test220919_anka-controller    "/bin/bash -c 'anka-…"   2 minutes ago       Up 2 minutes        0.0.0.0:8090->80/tcp     test220919_anka-controller_1
```

If the controller is crashing the status could be “restarting” or “failed”. Example for restarting state: “Restarting (1) 12 seconds ago”.

You can try to run `docker-compose up` without any parameters and this will output stdout for all services to the screen.

In case one of the services is crashing you could see the errors.Here is an example.

```bash
anka-controller_1   | E1128 15:17:16.952500       1 main.go:618] Could not build tls configuration:
anka-controller_1   | E1128 15:17:16.953204       1 main.go:619] open /whwhwhw/path: no such file or directory
test220919_anka-controller_1 exited with code 1
```
You can also take a look at the logs using `docker logs --tail 100 -f $CONTAINER_NAME`.

