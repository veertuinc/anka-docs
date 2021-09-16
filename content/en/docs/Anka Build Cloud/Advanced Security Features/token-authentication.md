---
date: 2019-07-03T22:24:47-05:00
title: "Configuring Token Authentication"
linkTitle: "Token Authentication"
weight: 1
description: >
  How to set up token based authentication for your build cloud services.
---

> **This guide requires an Anka Enterprise or Enterprise Plus license.**

Starting in 1.19.0 of the Anka Build Cloud, there are two options for securing the communication with the Build Cloud Controller & Registry:

1. Setting the Root Token, protecting the Controller API and UI.

2. Generating a user private key which is then used to request temporary session tokens. These session tokens allow access to the API to perform various tasks.

However, there are several license specific differences that should be noted before you begin:

- **Enterprise:** By default, any tokens you generate (root or user) always have full access to the API.
- **Enterprise Plus:** By default, _only_ the root token has full access to the API. User tokens _must be_ created with a group attached and permissions added for the group.

> We recommend disjoining all but one node. One node must stay joined to ensure the Build Cloud has the proper license attached. You can rejoin it later once you're configured and you've rejoined all of your other nodes.

---

## Protecting your cloud with a Root Token

Enabling root token authentication is a simple process. The root user has what we call "superuser" (full) access to the controller, API, etc.

### How to Configure

> **The root token must be at least 10 characters long.**

##### MacOS Package

Edit `/usr/local/bin/anka-controllerd` and add:

```bash
export ANKA_ENABLE_AUTH="true"
export ANKA_ROOT_TOKEN="{min10chartoken}"
```

##### Linux/Docker Package

> With our docker package, each service is split up into its own container. You can enable a root token for either the controller, registry, or both.

Edit the `docker-compose.yml` and add both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` environment variables:

{{< highlight dockerfile "hl_lines=16 17" >}}

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
   restart: always
   environment:
     ANKA_ENABLE_AUTH: "true"
     ANKA_ROOT_TOKEN: "0987654321"

. . .

{{</ highlight >}}

### Testing

If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear. 

![root token login](/images/anka-build-cloud/advanced-security-features/controller-root-token-login.png)

Enter the token you specified and ensure that it logs you in.

---

## Protecting your cloud with User Keys and Session Tokens

### How to Configure

##### MacOS Package

Edit `/usr/local/bin/anka-controllerd` and add:

```bash
export ANKA_ENABLE_AUTH="true"
export ANKA_ROOT_TOKEN="{min10chartoken}"
```

##### Linux/Docker Package

> With our docker package, each service is split up into its own container. You can enable a root token for either the controller, registry, or both.

Edit the `docker-compose.yml` and add both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` environment variables:

{{< highlight dockerfile "hl_lines=16 17" >}}

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
   restart: always
   environment:
     ANKA_ENABLE_AUTH: "true"
     ANKA_ROOT_TOKEN: "0987654321"

. . .

{{</ highlight >}}

### Testing

If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear. 

![root token login](/images/anka-build-cloud/advanced-security-features/controller-root-token-login.png)

Enter the token you specified and ensure that it logs you in.







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