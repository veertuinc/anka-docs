---
title: "Configuration Reference"
linkTitle: "Configuration Reference"
weight: 5
description: >
  Anka Build Cloud Configuration Reference
---

## Controller Configuration Reference

Configuring your Anka Build Cloud Controller & Registry to enable features or customize URLs has several methods available.

> If you're using the standalone Registry package, you'll need to use Flags/Options and edit your `/Library/LaunchDaemons/com.veertu.anka.registry.plist`, then restart it with `launchctl unload /Library/. . . && launchctl load /Library/. . .`

> **The .docker files in the docker/linux package and dockerhub tags include a mix of non-`ANKA_` ENVs and flags (in the ENTRYPOINT). While we work on simplifying the docker package to support the `ANKA_` environment variables, you may need to remove the flags and non-`ANKA_` ENVs from the .docker file. Using ANKA_LISTEN_ADDR for example while the ENTRYPOINT has `--listen_addr` defined will cause a problem.**

{{< tabpane >}}
{{< tab header="Environment Variables" >}}

## Environment Variables

Depending on the package you're using (native or docker), you can set ENV variables to modify the configuration of your controller and registry.
#### docker-compose.yml (docker)

```yml
  anka-controller:
    container_name: anka-controller
    build:
       context: .
       dockerfile: anka-controller.docker
    ports:
       - "8090:80"
       #- "8100:8100"
    volumes:
       - /Users/myUserName:/mnt/cert
    depends_on:
       - etcd
       - anka-registry
    restart: always
    environment:
      ANKA_REGISTRY_ADDR: "http://anka.registry:8089"
      ANKA_USE_HTTPS: "false"
      ANKA_SKIP_TLS_VERIFICATION: "false"
      ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      ANKA_ENABLE_AUTH: "false"
      # SSL + Cert Auth
      #ANKA_USE_HTTPS: "true"
      #ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      #ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      #ANKA_SKIP_TLS_VERIFICATION: "true"
      #ANKA_ENABLE_AUTH: "true"
      #ANKA_ROOT_TOKEN: "1111111111"
      #ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      #ANKA_CLIENT_CERT="/mnt/cert/anka-controller-crt.pem"
      #ANKA_CLIENT_CERT_KEY="/mnt/cert/anka-controller-key.pem"
  anka-registry:
    container_name: anka-registry
    build:
        context: .
        dockerfile: anka-registry.docker
    ports:
        - "8089:8089"
    restart: always
    volumes:
      - "/Library/Application Support/Veertu/Anka/registry:/mnt/vol"
      # SSL + Cert Auth | - /Users/myUser/mycerts:/mnt/cert
    # SSL + Cert Auth | environment:
      #ANKA_USE_HTTPS: "true"
      #ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      #ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      #ANKA_SKIP_TLS_VERIFICATION: "true"
      #ANKA_ENABLE_REGISTRY_AUTHORIZATION: "true"
      #ANKA_ENABLE_AUTH: "true"
      #ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      #ANKA_CLIENT_CERT="/mnt/cert/anka-controller-crt.pem"
      #ANKA_CLIENT_CERT_KEY="/mnt/cert/anka-controller-key.pem"
```

#### /usr/local/bin/anka-controllerd (native)

> You must comment out the export to disable

```bash
#!/bin/bash

export ANKA_STANDALONE="true"
export ANKA_LISTEN_ADDR=":8090"
export ANKA_DATA_DIR="/Library/Application Support/Veertu/Anka/anka-controller"
export ANKA_ENABLE_CENTRAL_LOGGING="true"
export ANKA_LOG_DIR="/Library/Logs/Veertu/AnkaController"

export ANKA_RUN_REGISTRY="true"
export ANKA_REGISTRY_BASE_PATH="/Library/Application Support/Veertu/Anka/registry"
export ANKA_REGISTRY_LISTEN_ADDRESS="anka.registry:8089"
export ANKA_ANKA_REGISTRY="http://anka.registry:8089"

# SSL + Cert Auth
#export ANKA_ANKA_REGISTRY="https://anka.registry:8089"
#export ANKA_USE_HTTPS="true"
#export ANKA_SKIP_TLS_VERIFICATION="true"
#export ANKA_SERVER_CERT="/Users/MyUser/anka-controller-crt.pem"
#export ANKA_SERVER_KEY="/Users/MyUser/anka-controller-key.pem"

#export ANKA_ENABLE_AUTH="true"
#export ANKA_ENABLE_REGISTRY_AUTHORIZATION="true"
#export ANKA_CA_CERT="/Users/MyUser/anka-ca-crt.pem"
#export ANKA_CLIENT_CERT="/Users/MyUser/anka-controller-crt.pem"
#export ANKA_CLIENT_CERT_KEY="/Users/MyUser/anka-controller-key.pem"
#export ANKA_ROOT_TOKEN="1111111111"

/Library/Application\ Support/Veertu/Anka/bin/anka-controller
```

### General and Common

> When editing the `/usr/local/bin/anka-controllerd`, be sure to use export when setting the ENV

| Name | Type | Description | Default Value | ENV |
| --- | :---: | --- | :---: | :---: |
| Version | bool | Prints controller version and exits | - | ANKA_VERSION | 
| External Registry address | string | Anka Registry external URL (http[s]://hostname:[port]). This is passed to the Nodes, so they can download (and start) VMs | (required) | ANKA_ANKA_REGISTRY |
| Configuration file | string | (DEPRECATED) Path to a configuration file in INI format. You can use the file with/without the command line parameters and env variables | - | ANKA_CONFIG |
| Listen address | string | Listen on this address (:80 is equivalent to 0.0.0.0:80). Use the format `[address]:port` | :80 | ANKA_LISTEN_ADDR |
| Local Registry Address | string | Anka Registry local address in format `http[s]://hostname:[port]`. This parameter is for situations where the Controller and Registry are on the same network. For example `http://locahost:8089` | - | ANKA_LOCAL_ANKA_REGISTRY |
| Number of concurrent workers | int | The number of concurrent workers processing node tasks | 2 | ANKA_NUM_WORKERS |
| Standalone mode | bool | Run an embedded ETCD server alongside the controller | false | ANKA_STANDALONE |
| ETCD endpoints | string | Comma-separated list of etcd addresses. These endpoints are used for the Application DB (instance, group, node information) and the Queue DB (if not defined separately with `ANKA_QUEUE_ETCD_ENDPOINTS`) | 127.0.0.1:2379 | ANKA_ETCD_ENDPOINTS |
| Queue ETCD endpoints | string | Comma-separated list of etcd addresses to use for only the Queue DB (task information). | 127.0.0.1:2379 | ANKA_QUEUE_ETCD_ENDPOINTS |
| ETCD defrag interval | duration | Defrag ETCD (all servers) at this interval (0 to disable) | 3h | ANKA_DEFRAG_DB_INTERVAL |
| Allow empty registry | bool | Allow controller to start without a 'Registry address' | false | ANKA_ALLOW_EMPTY_REGISTRY |
| Enable event logging | bool | Enables event logging. **`Requires a Enterprise Plus license and will show under the Controller's Logs section after the first instance is created.`** | false | ANKA_ENABLE_EVENT_LOGGING |
| Event log url | string | The URL to post events (in json format) | - | ANKA_EVENT_LOG_URL |
| Enable central logging | bool | Enables central logging | false | ANKA_ENABLE_CENTRAL_LOGGING |
| Push registry | string | Comma separated list of Registry addresses to use for push operations (saveImage/Jenkins cache building) | - | ANKA_PUSH_REGISTRY |
| Instance time out | duration | The time that instances stay in 'Terminated' state | 1m | ANKA_INSTANCE_TIME_OUT |
| Manage MAC addresses | bool | Let the controller manage VM MAC addresses to ensure uniqueness/prevent collision. **`Requires VM Templates/Tags be stored in your Registry in a stopped state (vs suspended).`** | false | ANKA_MANAGE_MAC_ADDRESSES |
| Clean MAC addresses interval | duration | Interval between cleanings of unused MAC addresses | 1h | ANKA_CLEAN_MAC_ADDRESS_INTERVAL |
| Limit MAC addresses to a range | string | Allows passing the range of mac addresses to use. `ANKA_MANAGE_MAC_ADDRESSES` must be set to true to use this option. Format: `<FROM>-<TO>` (example: 00:00:00:00:00:00-FF:FF:FF:FF:FF:FF) (In the example range, 00:00:00:00:00:00 and FF:FF:FF:FF:FF:FF will be included in the internal MAC address list) | - | ANKA_MAC_ADDR_RANGE |
| MAC address request retries | int | Times to retry to get mac address from the database before giving up and returning an error. | 100 | ANKA_MAC_ADDR_RANGE_MAX_RETRIES |
| MAC address range validation interval | duration | Interval to execute the mac address range validation. | 3h | ANKA_FILL_MAC_ADDRESS_RANGE_INTERVAL |

### Logging 

| Name | Type | Description | Default Value | ENV |
| --- | :---: | --- | :---: | :---: |
| Log level | int | Log level verbosity. Higher number means more verbose | 0 | ANKA_LOG_LEVEL |
| Log to stderr | bool | log to standard error instead of files | false | ANKA_LOGTOSTDERR |
| Log directory | string | Write log files in this directory | | ANKA_LOG_DIR |
| Also log to stderr| bool | Log to standard error as well as files | true | ANKA_ALSOLOGTOSTDERR |

### TLS

| Name | Type | Description | Default Value | ENV |  
| --- | :---: | --- | :---: | :---: |
Enable https | bool | Use https protocol for the controller portal/APIs. **Must pass this to enable TLS** | false | ANKA_USE_HTTPS 
CA certificate | string | Path to a CA cert to use for authenticating clients | - | ANKA_CA_CERT   
Root certificate | string | Similar to CA certificate | - | ANKA_ROOT_CERT 
Server certificate | string | Path to TLS server certificate | - | ANKA_SERVER_CERT
Server certificate key | string | Path to the server certificate's private key | - | ANKA_SERVER_KEY
Skip TLS verification | bool | Don't verify TLS certificates | false | ANKA_SKIP_TLS_VERIFICATION
Client certificate | string | Path to client certificate. The Controller will use this certificate when making http requests (mainly to the Registry). | - | ANKA_CLIENT_CERT
Client certificate key| string | Path to the client certificate's private key | - | ANKA_CLIENT_CERT_KEY
Client keystore| string | Path to a client keystore file in pkcs12 format. The Controller will use the certificate from this key store when making http requests (mainly to the Registry). | - | ANKA_CLIENT_KEYSTORE
Client keystore password | string | Password for the client keystore (optional). | - | ANKA_CLIENT_KEYPASS
Allowed TLS Cipher Suites | comma separated, strings | A list of cipher suites to use for tls. Options: tls_rsa_with_3des_ede_cbc_sha, tls_rsa_with_aes_128_cbc_sha, tls_rsa_with_aes_256_cbc_sha, tls_rsa_with_aes_128_gcm_sha256, tls_rsa_with_aes_256_gcm_sha384, tls_aes_128_gcm_sha256, tls_aes_256_gcm_sha384, tls_chacha20_poly1305_sha256, tls_ecdhe_ecdsa_with_aes_128_cbc_sha, tls_ecdhe_ecdsa_with_aes_256_cbc_sha, tls_ecdhe_rsa_with_3des_ede_cbc_sha, tls_ecdhe_rsa_with_aes_128_cbc_sha, tls_ecdhe_rsa_with_aes_256_cbc_sha, tls_ecdhe_ecdsa_with_aes_128_gcm_sha256, tls_ecdhe_ecdsa_with_aes_256_gcm_sha384, tls_ecdhe_rsa_with_aes_128_gcm_sha256, tls_ecdhe_rsa_with_aes_256_gcm_sha384, tls_ecdhe_rsa_with_chacha20_poly1305_sha256, tls_ecdhe_ecdsa_with_chacha20_poly1305_sha256 | None | ANKA_CIPHER_SUITES
Minimal TLS Version | string | The min tls version to use. Options: tls_1.0, tls_1.1, tls_1.2, tls_1.3 | None | ANKA_MIN_TLS_VERSION
Maximal TLS Version | string | The max tls version to use. Options: tls_1.0, tls_1.1, tls_1.2, tls_1.3 | None | ANKA_MAX_TLS_VERSION

### Built in Registry

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: |
Run registry | bool | Run the embedded Registry server | false | ANKA_RUN_REGISTRY
Registry listen address| string | Address for Registry to listen on (:8089 is equivalent to 0.0.0.0:8089). Use the format `[address]:port` | :8089 | ANKA_REGISTRY_LISTEN_ADDRESS
Registry base path | string | Path for registry's data | - | ANKA_REGISTRY_BASE_PATH
Registry access logs| bool | Enables registry access logs | false | ANKA_REGISTRY_ACCESS_LOGS
Enable registry authorization| | Enables authorization for the Registry | false | ANKA_ENABLE_REGISTRY_AUTHORIZATION

### Built in ETCD

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: |
Server name | string | Human readable name for ETCD server | anka-etcd | ANKA_NAME      
Data directory | string | Path to use for saving ETCD data | /tmp/etcd-data | ANKA_DATA_DIR  
Initial cluster| string | Initial cluster configuration for bootstrapping etcd server | anka-etcd=http://0.0.0.0:2380 | ANKA_INITIAL_CLUSTER
Listen peer urls| string | Comma separated URLs for ETCD server to server communication (when clustering ETCD) | http://0.0.0.0:2380 | ANKA_LISTEN_PEER_URLS
Initial advertise peer urls| string | Comma separated URLs for ETCD server to server communication to advertise | http://0.0.0.0:2380 | ANKA_INITIAL_ADVERTISE_PEER_URLS
Initial ETCD state| string | Initial ETCD cluster state ('new' or 'existing') | new | ANKA_INITIAL_CLUSTER_STATE
Initial ETCD token| string | Initial token for the ETCD cluster during bootstrap | etcd-server | ANKA_INITIAL_CLUSTER_TOKEN
Listen client urls| string | Comma separated URLs for ETCD to serve clients (Controller) | http://127.0.0.1:2379 | ANKA_LISTEN_CLIENT_URLS
Auto compaction mode| string | Auto compaction mode, either 'periodic' or 'revision'. | periodic | ANKA_AUTO_COMPACTION_MODE
Advertise client urls| string | Client urls for etcd server to advertise | http://127.0.0.1:2379 | ANKA_ADVERTISE_CLIENT_URLS
Compaction retention interval | string | Auto compaction retention length. 0 means disable auto compaction. | 30m | ANKA_AUTO_COMPACTION_RETENTION

### Authentication and Authorization 

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: | 
Enable authentication | bool | Enable authentication module. **Must pass this for authentication to work** | false | ANKA_ENABLE_AUTH
Root static token | string | A token to authenticate as super user | - | ANKA_ROOT_TOKEN
OpenId connect display name| string | Name of open id server to display in login page. The text will say "Login with X" | - | ANKA_OIDC_DISPLAY_NAME
OpenId connect provider url| string | Open ID connect provider url | - | ANKA_OIDC_PROVIDER_URL
OpenId connect  client id | string | Open ID connect client id | - | ANKA_OIDC_CLIENT_ID
OpenId connect  username claim| string | Open ID connect claim key to use for user name | name | ANKA_OIDC_USERNAME_CLAIM
OpenId connect groups claim| string | Open ID connect claim key to use for groups, | groups | ANKA_OIDC_GROUPS_CLAIM
Enable Etcd Authentication| bool | Use TLS certificates for authentication with etcd server. **Must pass this for etcd authentication to work** | false | ANKA_USE_ETCD_TLS
Etcd CA Cert| string | Path to CA certificate to be used when connecting to Etcd server | - | ANKA_ETCD_CA_CERT
Etcd Client Cert| string | Path to Etcd Client certificate to be used when connecting to Etcd server | - | ANKA_ETCD_CERT
Etcd Client Key| string | Path to Etcd Client Key to be used when connecting to Etcd server | - | ANKA_ETCD_CERT_KEY
Skip Etcd TLS verification | bool | Don't use TLS verification for Etcd Authentication | false | ANKA_SKIP_ETCD_TLS_VERIFICATION
Enable Etcd user login | bool | Enable Etcd user login when connecting to Etcd server | false | ANKA_USE_ETCD_LOGIN
Etcd Username | string | Etcd username to be used to login to Etcd server | - | ANKA_ETCD_USERNAME
Etcd Password | string | Etcd password to be used to login to Etcd server | - | ANKA_ETCD_PASSWORD

### Separate queue interface

> This is an advanced feature, it allows you to have a second http interface that will be used only by the cluster's Nodes

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: | 
Queue address | string | Setting this address will activate a separate http server that will only serve queue requests (only for Node communication). | - | ANKA_QUEUE_ADDR
Queue CA certificate | string | Path to a CA certificate to use for authenticating clients | - | ANKA_QUEUE_CA_CERT
Queue server certificate| string | Path to TLS server certificate | - | ANKA_QUEUE_SERVER_CERT
Queue server certificate key| string | Path to the server certificate's private key | - | ANKA_QUEUE_SERVER_KEY
Use queue TLS | | Enables queue tls | false | ANKA_USE_QUEUE_TLS
Enable queue auth| | Enables queue authentication/authorization | false | ANKA_ENABLE_QUEUE_AUTH

### Internal

> Parameters used internally. It's recommended to use the Default Values.

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: |
Clean process interval| duration | The interval to clean the queues (delete any tasks older than 24 hours), 0 to disable | 1h | ANKA_CLEAN_QUEUES_INTERVAL
allow cors | bool | If true adds Acces-Control-Allow-Origin to all routes | default | ANKA_ALLOW_CORS
Scheduler interval| duration | Interval for checking scheduled tasks | 30m | ANKA_SCHEDULER_INTERVAL
allowUnknownFlags| | (DEPRECATED) Don't terminate the app if ini file contains unknown flags. | default | ANKA_ALLOWUNKNOWNFLAGS
Dump flags | bool | Dumps values for all flags defined in the app into stdout in ini-compatible syntax and terminates the app. | false | ANKA_DUMPFLAGS 

{{< /tab >}}
{{< tab header="Flags / Options" >}}

---

## Flags / Options

Depending on the package you're using (native or docker), you can include flags to modify the configuration of your controller and registry. 

#### docker-compose.yml (docker)

```yml
  anka-controller:
    container_name: anka-controller
    build:
       context: .
       dockerfile: anka-controller.docker
    ports:
       - "80:80"
       # SSL + Cert Auth | - "443:80"
    # SSL + Cert Auth | volumes:
    #    - /Users/myUser/mycerts/:/mnt/cert
    depends_on:
       - etcd
       - anka-registry
    restart: always
    entrypoint: ["/bin/bash", "-c", "anka-controller --standalone --enable-central-logging --anka-registry http://anka.registry:8089 --etcd-endpoints etcd:2379 --log_dir /var/log/anka-controller --local-anka-registry http://anka-registry:8085"]
    # SSL + Cert Auth | entrypoint: ["/bin/bash", "-c", "anka-controller --standalone --enable-central-logging --anka-registry https://anka.registry:8089 --etcd-endpoints etcd:2379 --log_dir /var/log/anka-controller --local-anka-registry http://anka-registry:8085 --use-https --server-cert /mnt/cert/anka-controller-crt.pem --server-key /mnt/cert/anka-controller-key.pem --enable-auth --ca-cert /mnt/cert/anka-ca-crt.pem --enable-registry-authorization --skip-tls-verification --client-cert /mnt/cert/anka-controller-crt.pem --client-key /mnt/cert/anka-controller-key.pem --root-token 1111111111"]

  anka-registry:
    container_name: anka-registry
    build:
        context: .
        dockerfile: anka-registry.docker
    ports:
        - "8089:8089"
    restart: always
    volumes:
      - "/Library/Application Support/Veertu/Anka/registry:/mnt/vol"
      # SSL + Cert Auth | - /Users/myUser/mycerts:/mnt/cert
    # SSL + Cert Auth | environment:
      #ANKA_USE_HTTPS: "true"
      #ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      #ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      #ANKA_SKIP_TLS_VERIFICATION: "true"
      #ANKA_ENABLE_REGISTRY_AUTHORIZATION: "true"
      #ANKA_ENABLE_AUTH: "true"
      #ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      #ANKA_CLIENT_CERT="/mnt/cert/anka-controller-crt.pem"
      #ANKA_CLIENT_CERT_KEY="/mnt/cert/anka-controller-key.pem"
```

#### /usr/local/bin/anka-controllerd (native)

```bash
#!/bin/bash
/Library/Application\ Support/Veertu/Anka/bin/anka-controller \
--standalone \
--listen_addr ":8090" \
--run-registry \
--anka-registry "http://anka.registry:8089" \
--registry-listen-address ":8089" \
--enable-central-logging \
--log_dir "/Library/Logs/Veertu/AnkaController" \
--data-dir "/Library/Application Support/Veertu/Anka/anka-controller" \
--registry-base-path "/Library/Application Support/Veertu/Anka/registry" \
# SSL + Cert Auth
# --anka-registry "https://anka.registry:8089" \
# --use-https \
# --enable-auth \
# --root-token "1111111111" \
# --enable-registry-authorization \
# --skip-tls-verification \
# --ca-cert $CERT_FOLDER/anka-ca-crt.pem \
# --server-cert $CERT_FOLDER/anka-controller-crt.pem \
# --server-key $CERT_FOLDER/anka-controller-key.pem \
# --client-cert $CERT_FOLDER/anka-controller-crt.pem \
# --client-cert-key $CERT_FOLDER/anka-controller-key.pem
```
### General and Common

| Name | Type | Description | Default Value | flag / opt |
| --- | :---: | --- | :---: | :---: |
| Version | bool | Prints controller version and exits | - | `--version` | 
| External Registry address | string | Anka Registry external URL (http[s]://hostname:[port]). This is passed to the Nodes, so they can download (and start) VMs | **(required)** | `--anka-registry` |
| Configuration file | string | (DEPRECATED) Path to a configuration file in INI format. You can use the file with/without the command line parameters and env variables | - | `--config` |
| Listen address | string | Listen on this address (:80 is equivalent to 0.0.0.0:80). Use the format `[address]:port` | :80 | `--listen_addr` |
| Local Registry Address | string | Anka Registry local address in format `http[s]://hostname:[port]`. This parameter is for situations where the Controller and Registry are on the same network. For example `http://locahost:8089`. This is NOT used for Nodes. If not specified, External address is used. | - | `--local-anka-registry` |
| Number of concurrent workers | int | The number of concurrent workers processing node tasks | 2 | `--num-workers` |
| Standalone mode | bool | Run an embedded ETCD server alongside the controller | false | `--standalone` |
| ETCD endpoints | string | Comma-separated list of etcd addresses. These endpoints are used for the Application DB (instance, group, node information) and the Queue DB (if not defined separately with `ANKA_QUEUE_ETCD_ENDPOINTS`) | 127.0.0.1:2379 | `--etcd-endpoints` |
| Queue ETCD endpoints | string | Comma-separated list of etcd addresses to use for only the Queue DB (task information). | 127.0.0.1:2379 | `--queue-etcd-endpoints` |
| ETCD defrag interval | duration | Defrag ETCD (all servers) at this interval (0 to disable) | 3h | `--defrag-db-interval` |
| Allow empty registry | bool | Allow controller to start without a 'Registry address' | false | `--allow-empty-registry` |
| Enable event logging | bool | Enables event logging. **`Requires a Enterprise Plus license and will show under the Controller's Logs section after the first instance is created.`** | false | `--enable-event-logging` |
| Event log url | string | The URL to post events (in json format) | - | `--event-log-url` |
| Enable central logging | bool | Enables central logging | false | `--enable-central-logging` |
| Push registry | string | Comma separated list of Registry addresses to use for push operations | - | `--push-registry` |
| Instance time out | duration | The time that instances stay in 'Terminated' state | 1m | `--instance-time-out` |
| Manage MAC addresses | bool | Let the controller manage VM MAC addresses to ensure uniqueness/prevent collision. **`Requires VM Templates/Tags be stored in your Registry in a stopped state (vs suspended).`** | false | `--manage-mac-addresses` |
| Clean MAC addresses interval | duration | Interval between cleanings of unused MAC addresses | 1h | `--clean-mac-address-interval` |
| Limit MAC addresses to a range | string | Allows passing the range of mac addresses to use. `--manage-mac-addresses` must be set to true to use this option. Format: `<FROM>-<TO>` (example: 00:00:00:00:00:00-FF:FF:FF:FF:FF:FF) (In the example range, 00:00:00:00:00:00 and FF:FF:FF:FF:FF:FF will be included in the internal MAC address list) | - | `--mac-addr-range` |
| MAC address request retries | int | Times to retry to get mac address from the database before giving up and returning an error. | 100 | `--mac-addr-range-max-retries` |
| MAC address range validation interval | duration | Interval to execute the mac address range validation. | 3h | `--fill-mac-address-range-interval` |

> External Registry Address: Required | Nodes use the external URL
> Local Registry Address: Optional | Nodes do NOT use local URL | If not specified/empty, External URL is used
### Logging 

| Name | Type | Description | Default Value | flag / opt |
| --- | :---: | --- | :---: | :---: |
| Log level | int | Log level verbosity. Higher number means more verbose | 0 | `--log-level` |
| Log to stderr | bool | log to standard error instead of files | false | `--logtostderr` |
| Log directory | string | Write log files in this directory | | `--log_dir` |
| Also log to stderr| bool | Log to standard error as well as files | true | `--alsologtostderr` |

### TLS

| Name | Type | Description | Default Value | flag / opt |  
| --- | :---: | --- | :---: | :---: |
Enable https | bool | Use https protocol for the controller portal/APIs. **Must pass this to enable TLS** | false | `--use-https` 
CA certificate | string | Path to a CA cert to use for authenticating clients | - | `--ca-cert`   
Root certificate | string | Alias of CA certificate | - | `--root-cert`
Server certificate | string | Path to TLS server certificate | - | `--server-cert`
Server certificate key | string | Path to the server certificate's private key | - | `--server-key`
Skip TLS verification | bool | Don't verify TLS certificates | false | `--skip-tls-verification`
Client certificate | string | Path to client certificate. The Controller will use this certificate when making http requests (mainly to the Registry). | - | `--client-cert`
Client certificate key| string | Path to the client certificate's private key | - | `--client-cert-key`
Client keystore| string | Path to a client keystore file in pkcs12 format. The Controller will use the certificate from this key store when making http requests (mainly to the Registry). | - | `--client-keystore`
Client keystore password | string | Password for the client keystore (optional). | - | `--client-keypass`
Allowed TLS Cipher Suites | comma separated, strings | A list of cipher suites to use for tls. Options: tls_rsa_with_3des_ede_cbc_sha, tls_rsa_with_aes_128_cbc_sha, tls_rsa_with_aes_256_cbc_sha, tls_rsa_with_aes_128_gcm_sha256, tls_rsa_with_aes_256_gcm_sha384, tls_aes_128_gcm_sha256, tls_aes_256_gcm_sha384, tls_chacha20_poly1305_sha256, tls_ecdhe_ecdsa_with_aes_128_cbc_sha, tls_ecdhe_ecdsa_with_aes_256_cbc_sha, tls_ecdhe_rsa_with_3des_ede_cbc_sha, tls_ecdhe_rsa_with_aes_128_cbc_sha, tls_ecdhe_rsa_with_aes_256_cbc_sha, tls_ecdhe_ecdsa_with_aes_128_gcm_sha256, tls_ecdhe_ecdsa_with_aes_256_gcm_sha384, tls_ecdhe_rsa_with_aes_128_gcm_sha256, tls_ecdhe_rsa_with_aes_256_gcm_sha384, tls_ecdhe_rsa_with_chacha20_poly1305_sha256, tls_ecdhe_ecdsa_with_chacha20_poly1305_sha256 | None | `--cipher-suites`
Minimal TLS Version | string | The min tls version to use. Options: tls_1.0, tls_1.1, tls_1.2, tls_1.3 | None | `--min-tls-version`
Maximal TLS Version | string | The max tls version to use. Options: tls_1.0, tls_1.1, tls_1.2, tls_1.3 | None | `--max-tls-version`

### Built in Registry

| Name | Type | Description | Default Value | flag / opt | 
| --- | :---: | --- | :---: | :---: |
Run registry | bool | Run the embedded Registry server | false | `--run-registry`
Registry listen address| string | Address for Registry to listen on (:8089 is equivalent to 0.0.0.0:8089). Use the format `[address]:port` | :8089 | `--registry-listen-address`
Registry base path | string | Path for registry's data | - | `--registry-base-path`
Registry access logs| bool | Enables registry access logs | false | `--registry-access-logs`
Enable registry authorization| | Enables authorization for the Registry | false | `--enable-registry-authorization`

### Built in ETCD

| Name | Type | Description | Default Value | flag / opt | 
| --- | :---: | --- | :---: | :---: |
Server name | string | Human readable name for ETCD server | anka-etcd | `--name`
Data directory | string | Path to use for saving ETCD data | /tmp/etcd-data | `--data-dir` 
Initial cluster| string | Initial cluster configuration for bootstrapping etcd server | anka-etcd=http://0.0.0.0:2380 | `--initial-cluster`
Listen peer urls| string | Comma separated URLs for ETCD server to server communication (when clustering ETCD) | http://0.0.0.0:2380 | `--listen-peer-urls`
Initial advertise peer urls| string | Comma separated URLs for ETCD server to server communication to advertise | http://0.0.0.0:2380 | `--initial-advertise-peer-urls`
Initial ETCD state| string | Initial ETCD cluster state ('new' or 'existing') | new | `--initial-cluster-state`
Initial ETCD token| string | Initial token for the ETCD cluster during bootstrap | etcd-server | `--initial-cluster-token`
Listen client urls| string | Comma separated URLs for ETCD to serve clients (Controller) | http://127.0.0.1:2379 | `--listen-client-urls`
Auto compaction mode| string | Auto compaction mode, either 'periodic' or 'revision'. | periodic | `--auto-compaction-mode`
Advertise client urls| string | Client urls for etcd server to advertise | http://127.0.0.1:2379 | `--advertise-client-urls`
Compaction retention interval | string | Auto compaction retention length. 0 means disable auto compaction. | 30m | `--auto-compaction-retention`

### Authentication and Authorization 

| Name | Type | Description | Default Value | flag / opt | 
| --- | :---: | --- | :---: | :---: |
Anable authentication | bool | Enable authentication module. **Must pass this for authentication to work** | false | `--enable-auth`
Root static token | string | A token to authenticate as super user | - | `--root-token`
OpenId connect display name| string | Name of open id server to display in login page. The text will say "Login with X" | - | `--oidc-display-name`
OpenId connect provider url| string | Open ID connect provider url | - | `--oidc-provider-url`
OpenId connect  client id | string | Open ID connect client id | - | `--oidc-client-id`
OpenId connect  username claim| string | Open ID connect claim key to use for user name | name | `--oidc-username-claim`
OpenId connect groups claim| string | Open ID connect claim key to use for groups, | groups | `--oidc-groups-claim`
Enable Etcd Authentication| bool | Use TLS certificates for authentication with etcd server. **Must pass this for etcd authentication to work** | false | `--use-etcd-tls`
Etcd CA Cert| string | Path to CA certificate to be used when connecting to Etcd server | - | `--etcd-ca-cert`
Etcd Client Cert| string | Path to Etcd Client certificate to be used when connecting to Etcd server | - | `--etcd-cert`
Etcd Client Key| string | Path to Etcd Client Key to be used when connecting to Etcd server | - | `--etcd-cert-key`
Skip Etcd TLS verification | bool | Don't use TLS verification for Etcd Authentication | false | `--skip-etcd-tls-verification`
Enable Etcd user login | bool | Enable Etcd user login when connecting to Etcd server | false | `--use-etcd-login`
Etcd Username | string | Etcd username to be used to login to Etcd server | - | `--etcd-username`
Etcd Password | string | Etcd password to be used to login to Etcd server | - | `--etcd-password`

### Separate queue interface

> This is an advanced feature, it allows you to have a second http interface that will be used only by the cluster's Nodes

| Name | Type | Description | Default Value | flag / opt | 
| --- | :---: | --- | :---: | :---: |
Queue address | string | Setting this address will activate a separate http server that will only serve queue requests (only for Node communication). | - | `--queue-addr`
Queue CA certificate | string | Path to a CA certificate to use for authenticating clients | - | `--queue-ca-cert`
Queue server certificate| string | Path to TLS server certificate | - | `--queue-server-cert`
Queue server certificate key| string | Path to the server certificate's private key | - | `--queue-server-key`
Use queue TLS | | Enables queue tls | false | `--use-queue-tls`
Enable queue auth| | Enables queue authentication/authorization | false | `--enable-queue-auth`

### Internal

> Parameters used internally. It's recommended to use the Default Values.

| Name | Type | Description | Default Value | flag / opt | 
| --- | :---: | --- | :---: | :---: |
Clean process interval| duration | The interval to clean the queues (delete any tasks older than 24 hours), 0 to disable | 1h | `--clean-queues-interval`
allow cors | bool | If true adds Acces-Control-Allow-Origin to all routes | default | `--allow-cors`
Scheduler interval| duration | Interval for checking scheduled tasks | 30m | `--scheduler-interval`
allowUnknownFlags| | (DEPRECATED) Don't terminate the app if ini file contains unknown flags. | default | `--allowUnknownFlags`
Dump flags | bool | Dumps values for all flags defined in the app into stdout in ini-compatible syntax and terminates the app. | false | `--dumpflags`

{{< /tab >}}
{{< /tabpane >}}
