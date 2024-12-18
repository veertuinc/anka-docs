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

#### docker-compose.yml (docker)

```yml
version: '2'
services:
  anka-controller:
    build:
       context: controller
    ports:
       - "80:80"
    ######   EDIT HERE FOR TLS  ########
    # volumes:
      # Path to ssl certificates directory 
      # - ****EDIT_ME****:/mnt/cert
    depends_on:
       - etcd
       - anka-registry
    environment:
      ANKA_ETCD_ENDPOINTS: etcd:2379
      ANKA_LISTEN_ADDR: :80
      ANKA_LOG_DIR: /var/log/anka-controller # not to be confused with ANKA_LOGS_DIR
      ANKA_LOCAL_ANKA_REGISTRY: http://anka-registry:8089
      ANKA_ENABLE_CENTRAL_LOGGING: "true"
      ANKA_ANKA_REGISTRY: *******EDIT-ME********  # This URL must be reachable by your Anka nodes
      # https://docs.veertu.com/anka/anka-build-cloud/configuration-reference/#configuration-envs
    restart: always

  anka-registry:
    build:
      context: registry
    ports:
      - "8089:8089"
    restart: always
    environment:
      ANKA_BASE_PATH: /mnt/vol
      ANKA_LISTEN_ADDR: :8089
      ANKA_LOG_DIR: /var/log/anka-registry # where the registry stores its own logs, not central logs
      ANKA_LOGS_DIR: central-logs # becomes ${ANKA_BASE_PATH}/central-logs; does not support absolute paths
      # https://docs.veertu.com/anka/anka-build-cloud/configuration-reference/#configuration-envs
    volumes:
      ######   EDIT HERE  ########
      # Path to registry data folder.
      # VM data files and logs will be saved in this folder
      # - ****EDIT_ME****:/mnt/vol

      # Path to ssl certificates directory 
      # - ****EDIT_ME****:/mnt/cert

  etcd:
    build:
      context: etcd
    volumes:
      - /var/etcd-data:/etcd-data
    environment:
      ETCD_DATA_DIR: /etcd-data
      ETCD_LISTEN_CLIENT_URLS: http://0.0.0.0:2379
      ETCD_ADVERTISE_CLIENT_URLS: http://0.0.0.0:2379
      ETCD_LISTEN_PEER_URLS: http://0.0.0.0:2380
      ETCD_INITIAL_ADVERTISE_PEER_URLS: http://0.0.0.0:2380
      ETCD_INITIAL_CLUSTER: my-etcd=http://0.0.0.0:2380
      ETCD_INITIAL_CLUSTER_TOKEN: my-etcd-token
      ETCD_INITIAL_CLUSTER_STATE: new
      ETCD_AUTO_COMPACTION_RETENTION: 30m
      ETCD_AUTO_COMPACTION_MODE: periodic
      ETCD_NAME: my-etcd
    restart: always
```

#### /usr/local/bin/anka-controllerd (native)

{{< hint warning >}}
When editing the /usr/local/bin/anka-controllerd, be sure to use export when setting the ENV.
{{< /hint >}}

```bash
#!/usr/bin/env bash

export ANKA_STANDALONE="true" # If false, will disable etcd
export ANKA_LISTEN_ADDR=":80"
export ANKA_DATA_DIR="/Library/Application Support/Veertu/Anka/anka-controller"
export ANKA_ENABLE_CENTRAL_LOGGING="true"
export ANKA_LOG_DIR="/Library/Logs/Veertu/AnkaController"

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

{{< include file="_partials/anka-build-cloud/configuration-reference/controller/controller.md" >}}

{{< include file="_partials/anka-build-cloud/configuration-reference/registry/registry.md" >}}
