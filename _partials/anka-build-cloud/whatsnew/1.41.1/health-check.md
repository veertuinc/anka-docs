---
---

{{< hint warning >}}
This feature is only available in 1.41.1 or later.
{{< /hint >}}


The health check endpoints are available at `/livez` and `/readyz`. They are accessible regardless of authorization.

1. The `/livez` endpoint returns a 200 OK response if the service is alive.

- This is useful for `livenessProbes` in Kubernetes.

2. The `/readyz` endpoint returns a 200 OK response if the service is not only up but also ready to serve requests.

- This is useful for `readinessProbes` in Kubernetes.
- Docker users should rely on `/readyz` for their health checks. You can find an example inside of the 1.41.1 linux/docker package's `docker-compose-v3.yml` file.

```bash
version: '3.8'
services:
  anka-controller:
    build:
      context: controller
    ports:
      - "80:80"
    #volumes:
    #  - ****EDIT_ME****:/mnt/cert # Path to ssl certificates directory; don't forget to adjust healthcheck
    depends_on:
      etcd:
        condition: service_healthy
      anka-registry:
        condition: service_healthy
    environment: # https://docs.veertu.com/anka/anka-build-cloud/configuration-reference/#configuration-envs
      ANKA_ETCD_ENDPOINTS: 'etcd:2379'
      ANKA_LISTEN_ADDR: ':80'
      ANKA_LOG_DIR: '/var/log/anka-controller'
      ANKA_LOCAL_ANKA_REGISTRY: 'http://anka-registry:8089'
      ANKA_ENABLE_CENTRAL_LOGGING: "true"
      #ANKA_ANKA_REGISTRY: *******EDIT-ME********  # This URL must be reachable by your Anka nodes
    restart: always
    healthcheck:
      test: [ "CMD", "curl", "-f", "http://localhost:80/readyz" ]
      interval: 5s
      timeout: 30s
      start_period: 0s
      retries: 3

  anka-registry:
    build:
      context: registry
    ports:
      - "8089:8089"
    environment: # https://docs.veertu.com/anka/anka-build-cloud/configuration-reference/#configuration-envs
      ANKA_BASE_PATH: /mnt/vol
      ANKA_LISTEN_ADDR: :8089
    #volumes:
    #  Path to registry data folder.
    #  VM data files and logs will be saved in this folder
    #  - ****EDIT_ME****:/mnt/vol

    #  Path to ssl certificates directory
    #  - ****EDIT_ME****:/mnt/cert # Path to ssl certificates directory; don't forget to adjust healthcheck
    restart: always
    healthcheck:
      test: [ "CMD", "curl", "-f", "http://localhost:8089/readyz" ]
      interval: 5s
      timeout: 30s
      start_period: 0s
      retries: 3

  etcd:
    build:
      context: etcd
    volumes:
      - '/var/etcd-data:/etcd-data'
    environment:
      ETCD_DATA_DIR: '/etcd-data'
      ETCD_LISTEN_CLIENT_URLS: 'http://0.0.0.0:2379'
      ETCD_ADVERTISE_CLIENT_URLS: 'http://0.0.0.0:2379'
      ETCD_LISTEN_PEER_URLS: 'http://0.0.0.0:2380'
      ETCD_INITIAL_ADVERTISE_PEER_URLS: 'http://0.0.0.0:2380'
      ETCD_INITIAL_CLUSTER: 'my-etcd=http://0.0.0.0:2380'
      ETCD_INITIAL_CLUSTER_TOKEN: 'my-etcd-token'
      ETCD_INITIAL_CLUSTER_STATE: 'new'
      ETCD_AUTO_COMPACTION_RETENTION: '30m'
      ETCD_AUTO_COMPACTION_MODE: 'periodic'
      ETCD_NAME: 'my-etcd'
    restart: always
    healthcheck:
      test: [ "CMD", "curl", "-f", "http://localhost:2379/health" ]
      interval: 5s
      timeout: 30s
      start_period: 0s
      retries: 3
```

