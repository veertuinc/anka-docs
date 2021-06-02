---
date: 2019-07-03T22:24:47-05:00
title: "Configuring Root Token Authentication"
linkTitle: "Root Token Authentication"
weight: 1
description: >
  How to set up root token based authentication for your Controller dashboard.
---

> This guide requires an Anka Enterprise (or higher) license.

Enabling root token authentication is a simple process. The root user has what we call "superuser" (full) access to the controller and various features.

> We recommend disjoining your nodes before performing these steps

### Configuring

#### Mac Controller & Registry

Edit `/usr/local/bin/anka-controllerd` and append `--enable-auth --root-token $ROOT_TOKEN` to the end of the **$CONTROLLER_BIN** line.

#### Linux/Docker Controller & Registry

Edit `docker-compose.yml` and under the `anka-controller` service uncomment **ENABLE_AUTH** and **ROOT_TOKEN**, then modify **ROOT_TOKEN** with the token you want:

> **The root token should be at least 10 characters long**

{{< highlight dockerfile "hl_lines=17 18" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "80:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:
     ENABLE_AUTH:            --enable-auth 
     ROOT_TOKEN:             --root-token 0987654321

. . .

{{</ highlight >}}

### Testing Controller Dashboard authentication

If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear. Enter the token you specified and ensure that it logs you in.


### Joining Nodes

If you're using root token auth for your Controller UI without certificate authentication, Nodes will no longer be able to connect to port 80 (or whatever port you've set) when running `ankacluster join`. You'll need to [setup an interface for them to communicate]({{< relref "docs/Anka Build Cloud/configuration-reference#separate-queue-interface" >}}).

#### Mac Controller & Registry

Edit `/usr/local/bin/anka-controllerd` and append `--queue-addr ":8100"` to the end of the **$CONTROLLER_BIN** line.

#### Linux/Docker Controller & Registry

{{< highlight dockerfile "hl_lines=9 20" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "80:80"
      - "8100:8100"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:
     ENABLE_AUTH:            --enable-auth 
     ROOT_TOKEN:             --root-token 0987654321
     ANKA_QUEUE_ADDR: ":8100"

. . .

{{</ highlight >}}

> You must have at least one node with a Enterprise or higher license joined to the Controller for these features to work.

Then, join your Nodes and skip tests:

```bash
❯ sudo ankacluster join --skip-tests http://anka.controller:8100
Tests skipped
Cluster join success
```

You can also setup certificate authentication/authorization for the queue interface independent of the Controller.