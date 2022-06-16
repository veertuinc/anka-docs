---
title: "Configuration Reference"
linkTitle: "Configuration Reference"
weight: 5
description: >
  Anka Build Cloud Configuration Reference
---

## Controller Configuration Reference

Configuring your Anka Build Cloud Controller & Registry to enable features or customize URLs has several methods available.

## Environment Variables

Depending on the package you're using (native or docker), you can set ENV variables to modify the configuration of your controller and registry.

{{< hint info >}}
Our default docker package will use .env files to store the configuration ENVs. Both that and the below example are valid ways to configure the Anka Build Cloud.
{{< /hint >}}

#### docker-compose.yml (docker)

```yml
version: '2'
services:
  anka-controller:
    container_name: anka-controller
    build:
      context: controller
    ports:
      - "80:80"
    # volumes:
    #   - /Users/myUserName:/mnt/cert
    depends_on:
      - etcd
      - anka-registry
    restart: always
    environment:
      ANKA_ANKA_REGISTRY: "http://anka.registry:8089"
      # SSL + Cert Auth
      # ANKA_USE_HTTPS: "true"
      # ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      # ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      # ANKA_SKIP_TLS_VERIFICATION: "true"
      # ANKA_ENABLE_AUTH: "true"
      # ANKA_ROOT_TOKEN: "1111111111"
      # ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      # ANKA_CLIENT_CERT="/mnt/cert/anka-controller-crt.pem"
      # ANKA_CLIENT_CERT_KEY="/mnt/cert/anka-controller-key.pem"
  anka-registry:
    container_name: anka-registry
    build:
      context: registry
    ports:
      - "8089:8089"
    restart: always
    volumes:
      - "/Library/Application Support/Veertu/Anka/registry:/mnt/vol"
    # environment:  
      # SSL + Cert Auth
      # ANKA_USE_HTTPS: "true"
      # ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      # ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      # ANKA_SKIP_TLS_VERIFICATION: "true"
      # ANKA_ENABLE_AUTH: "true"
      # ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      # ANKA_CLIENT_CERT="/mnt/cert/anka-controller-crt.pem"
      # ANKA_CLIENT_CERT_KEY="/mnt/cert/anka-controller-key.pem"
  etcd:
    build:
      context: etcd
    volumes:
      - /var/etcd-data:/etcd-data
    environment:
      ETCD_DATA_DIR: "/etcd-data"
      ETCD_LISTEN_CLIENT_URLS: "http://0.0.0.0:2379"
      ETCD_ADVERTISE_CLIENT_URLS: "http://0.0.0.0:2379"
      ETCD_LISTEN_PEER_URLS: "http://0.0.0.0:2380"
      ETCD_INITIAL_ADVERTISE_PEER_URLS: "http://0.0.0.0:2380"
      ETCD_INITIAL_CLUSTER: "my-etcd=http://0.0.0.0:2380"
      ETCD_INITIAL_CLUSTER_TOKEN: "my-etcd-token"
      ETCD_INITIAL_CLUSTER_STATE: "new"
      ETCD_AUTO_COMPACTION_RETENTION: "1"
      ETCD_NAME: "my-etcd"
    restart: always
```

#### /usr/local/bin/anka-controllerd (native)

{{< hint warning >}}
When editing the /usr/local/bin/anka-controllerd, be sure to use export when setting the ENV.
{{< /hint >}}

```bash
#!/bin/bash

export ANKA_STANDALONE="true"
export ANKA_LISTEN_ADDR=":80"
export ANKA_DATA_DIR="/Library/Application Support/Veertu/Anka/anka-controller"
export ANKA_ENABLE_CENTRAL_LOGGING="true"
export ANKA_LOG_DIR="/Library/Logs/Veertu/AnkaController"

export ANKA_RUN_REGISTRY="true"
export ANKA_ALLOW_EMPTY_REGISTRY="true"
export ANKA_REGISTRY_BASE_PATH="/Library/Application Support/Veertu/Anka/registry"
export ANKA_REGISTRY_LISTEN_ADDRESS="anka.registry:8089"

# SSL + Cert Auth
# export ANKA_USE_HTTPS="true"
# export ANKA_SKIP_TLS_VERIFICATION="true"
# export ANKA_SERVER_CERT="/Users/nathanpierce/anka-build-cloud-certs/anka-controller-crt.pem"
# export ANKA_SERVER_KEY="/Users/nathanpierce/anka-build-cloud-certs/anka-controller-key.pem"

# export ANKA_ENABLE_AUTH="true"
# export ANKA_CA_CERT="/Users/nathanpierce/anka-build-cloud-certs/anka-ca-crt.pem"
# export ANKA_CLIENT_CERT="/Users/nathanpierce/anka-build-cloud-certs/anka-controller-crt.pem"
# export ANKA_CLIENT_CERT_KEY="/Users/nathanpierce/anka-build-cloud-certs/anka-controller-key.pem"
# export ANKA_ROOT_TOKEN="1111111111"


${ANKA_USE_HTTPS:-false} && SCHEME="https://" || SCHEME="http://"

export ANKA_ANKA_REGISTRY="${SCHEME}anka.registry:8089"

/Library/Application\ Support/Veertu/Anka/bin/anka-controller
```

---

{{< include file="_partials/Anka Build Cloud/configuration-reference/controller/controller.md" >}}

{{< include file="_partials/Anka Build Cloud/configuration-reference/registry/registry.md" >}}
