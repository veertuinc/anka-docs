---
date: 2019-07-03T22:24:47-05:00
title: "Configuring Root Token authentication"
linkTitle: "Root Token authentication"
weight: 1
description: >
  How to set up root token based authentication for your Controller dashboard.
---

> This guide requires an Anka Enterprise (or higher) license.

Enabling root token authentication is a simple process. The root user has what we call "superuser" (full) access to the controller and various features. _Note that the root token should be at least 10 characters long._

### Configuring for the Mac Controller & Registry

Edit `/usr/local/bin/anka-controllerd` and append `--root-token $ROOT_TOKEN` to the end of the **$CONTROLLER_BIN** line.

### Configuring for the Linux/Docker Controller & Registry

Edit `docker-compose.yml` and modify the environment variable **ROOT_TOKEN**:

{{< highlight dockerfile "hl_lines=25" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "443:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:     
     USE_HTTPS:              --use-https  
     # Server certificate pem
     SERVER_CERT:            --server-cert /mnt/cert/anka-controller-crt.pem
     # Server private key pem
     SERVER_KEY:             --server-key /mnt/cert/anka-controller-key.pem
     ENABLE_AUTH:            --enable-auth 
     CA_CERT:                --ca-cert /mnt/cert/anka-ca-crt.pem
     ROOT_TOKEN:             --root-token 0987654321

. . .

{{</ highlight >}}

### Testing Controller Dashboard authentication

If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear. Enter the token you specified and ensure that it logs you in.