
---
title: "Configuration Reference"
linkTitle: "Configuration Reference"
weight: 9
description: >
  Anka Build Cloud configuration options.
draft: true
---



## Controller configuration parameters

### General and Common

 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
version        |      bool       | Prints controller version and exits                          | -      | --version         | version         | ANKA_VERSION   
Registry address | string           | Anka Registry external URL (http[s]://hostname:[port]). This is passed to the Nodes, so they can download (and start) VMs | (required)      | --anka-registry   | anka_registry   | ANKA_ANKA_REGISTRY
Configuration file    | string           | Path to a configuration file in INI format. You can use the file with/without the command line parameters and env variables  | -      | --config          | config          | ANKA_CONFIG    
Listen address    | string           | Listen on this address (:80 is equivalent to 0.0.0.0:80). Use the format `[address]:port`                      | :80     | --listen_addr     | listen_addr     | ANKA_LISTEN_ADDR
Local Registry Address | string           | Anka Registry local address in format `http[s]://hostname:[port]`. This parameter is for situations where the Controller and Registry are on the same network. For example `http://locahost:8089`  | -      | --local-anka-registry | local_anka_registry | ANKA_LOCAL_ANKA_REGISTRY
Number of concurrent workers   | int   | The number of concurrent workers processing node tasks | 2      | --num-workers     | num_workers     | ANKA_NUM_WORKERS
Standalone mode     |    bool       | Run an embedded ETCD server alongside the controller   | false      | --standalone      | standalone      | ANKA_STANDALONE
ETCD endpoints | string      | Comma separated list of etcd hosts  | 127.0.0.1:2379   | --etcd-endpoints  | etcd_endpoints  | ANKA_ETCD_ENDPOINTS
Allow empty registry|  bool     | Allow controller to start without a 'Registry address' | false      | --allow-empty-registry | allow_empty_registry | ANKA_ALLOW_EMPTY_REGISTRY
Enable event logging|  bool            | Enables event logging                                | false      | --enable-event-logging | enable_event_logging | ANKA_ENABLE_EVENT_LOGGING
Event log url  | string           | The URL to post events (in json format)                    | -      | --event-log-url   | event_log_url   | ANKA_EVENT_LOG_URL
Enable central logging |      bool  | Enables central logging                              | false      | --enable-central-logging | enable_central_logging | ANKA_ENABLE_CENTRAL_LOGGING
Push registry  | string            | Comma separated list of Registry addresses to use for push operations | -      | --push-registry   | push_registry   | ANKA_PUSH_REGISTRY
ETCD defrag interval| duration         | Defrag ETCD (all servers) in this interval. Pass 0 to disable  | 3h | --defrag-db-interval | defrag_db_interval | ANKA_DEFRAG_DB_INTERVAL
Instance time out| duration         | The time that instances stay in 'Terminated' state | 1m      | --instance-time-out | instance_time_out | ANKA_INSTANCE_TIME_OUT

### Logging 

 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
Log level   | int         | Log level verbosity. Higher number means more verbose      | 0      | --v               | v               | ANKA_V         
Log to stderr    |  bool   | log to standard error instead of files   | false      | --logtostderr     | logtostderr     | ANKA_LOGTOSTDERR
Log directory     | string           | Write log files in this directory              | -      | --log_dir         | log_dir         | ANKA_LOG_DIR   
Also log to stderr|   bool       | Log to standard error as well as files    | true      | --alsologtostderr | alsologtostderr | ANKA_ALSOLOGTOSTDERR

### TLS

 Name       | Type        |   Description               | Default value | command line | ini    | env  
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
Enable https      |  bool     | Use https protocol for the controller portal/APIs. **Must pass this to enable TLS**            | false      | --use-https       | use_https       | ANKA_USE_HTTPS 
CA certificate        | string           | Path to a CA cert to use for authenticating clients        | -      | --ca-cert         | ca_cert         | ANKA_CA_CERT   
Root certificate      | string           | Similar to CA certificate                                  | -      | --root-cert       | root_cert       | ANKA_ROOT_CERT 
Server certificate    | string           |  Path to TLS server certificate                | -      | --server-cert     | server_cert     | ANKA_SERVER_CERT
Server certificate key     | string           | Path to the server certificate's private key               | -      | --server-key      | server_key      | ANKA_SERVER_KEY
Skip TLS verification|   bool      | Don't verify TLS certificates        | false      | --skip-tls-verification | skip_tls_verification | ANKA_SKIP_TLS_VERIFICATION
Client certificate    | string           | Path to client certificate. The Controller will use this certificate when making http requests (mainly to the Registry).        | -      | --client-cert     | client_cert     | ANKA_CLIENT_CERT
Client certificate key| string           | Path to the client certificate's private key                  | -      | --client-cert-key | client_cert_key | ANKA_CLIENT_CERT_KEY
Client keystore| string           | Path to a client keystore file in pkcs12 format. The Controller will use the certificate from this key store when making http requests (mainly to the Registry).        | -      | --client-keystore | client_keystore | ANKA_CLIENT_KEYSTORE
Client keystore password | string           | Password for the client keystore (optional).            | -      | --client-keypass  | client_keypass  | ANKA_CLIENT_KEYPASS



### Built in Registry

 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
Run registry   |    bool      | Run the embedded Registry server               | false      | --run-registry    | run_registry    | ANKA_RUN_REGISTRY
Registry listen address| string           | Address for Registry to listen on (:8089 is equivalent to 0.0.0.0:8089). Use the format `[address]:port`          | :8089      | --registry-listen-address | registry_listen_address | ANKA_REGISTRY_LISTEN_ADDRESS
Registry base path | string           | Path for registry's data                                | -      | --registry-base-path | registry_base_path | ANKA_REGISTRY_BASE_PATH
Registry access logs|      bool  | Enables registry access logs                                 | false      | --registry-access-logs | registry_access_logs | ANKA_REGISTRY_ACCESS_LOGS
Enable registry authorization|                  | Enables authorization for the Registry                                        | false      | --enable-registry-authorization | enable_registry_authorization | ANKA_ENABLE_REGISTRY_AUTHORIZATION


### Built in ETCD

 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
Server name           | string           | Human readable name for ETCD server     | anka-etcd     | --name            | name            | ANKA_NAME      
Data directory       | string           | Path to use for saving ETCD data       | /tmp/etcd-data      | --data-dir        | data_dir        | ANKA_DATA_DIR  
Initial cluster| string           | Initial cluster configuration for bootstrapping etcd server | anka-etcd=http://0.0.0.0:2380      | --initial-cluster | initial_cluster | ANKA_INITIAL_CLUSTER
Listen peer urls| string  | Comma separated URLs for ETCD server to server communication (when clustering ETCD) | http://0.0.0.0:2380      | --listen-peer-urls | listen_peer_urls | ANKA_LISTEN_PEER_URLS
Initial advertise peer urls| string  | Comma separated URLs for ETCD server to server communication to advertise  |  http://0.0.0.0:2380      | --initial-advertise-peer-urls | initial_advertise_peer_urls | ANKA_INITIAL_ADVERTISE_PEER_URLS
Initial ETCD state| string           | Initial ETCD cluster state ('new' or 'existing')  | new      | --initial-cluster-state | initial_cluster_state | ANKA_INITIAL_CLUSTER_STATE
Initial ETCD token| string           | Initial token for the ETCD cluster during bootstrap | etcd-server      | --initial-cluster-token | initial_cluster_token | ANKA_INITIAL_CLUSTER_TOKEN
Listen client urls| string    | Comma separated URLs for ETCD to serve clients (Controller) | http://127.0.0.1:2379      | --listen-client-urls | listen_client_urls | ANKA_LISTEN_CLIENT_URLS
Auto compaction mode| string           | Auto compaction mode, either 'periodic' or 'revision'. | periodic      | --auto-compaction-mode | auto_compaction_mode | ANKA_AUTO_COMPACTION_MODE
Advertise client urls| string            | Client urls for etcd server to advertise  | http://127.0.0.1:2379      | --advertise-client-urls | advertise_client_urls | ANKA_ADVERTISE_CLIENT_URLS
Compaction retention interval | string           | Auto compaction retention length. 0 means disable auto compaction. | 30m      | --auto-compaction-retention | auto_compaction_retention | ANKA_AUTO_COMPACTION_RETENTION

### Authentication and Authorization 


 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
Anable authentication  |  bool   | Enable authentication module. **Must pass this for authentication to work**                                 | false      | --enable-auth     | enable_auth     | ANKA_ENABLE_AUTH
Root static token     | string           | A token to authenticate as super user                   | -      | --root-token      | root_token      | ANKA_ROOT_TOKEN
OpenId connect display name| string           | Name of open id server to display in login page. The text will say "Login with X"                                |  -      | --oidc-display-name | oidc_display_name | ANKA_OIDC_DISPLAY_NAME
OpenId connect provider url| string  | Open ID connect provider url                                 | -      | --oidc-provider-url | oidc_provider_url | ANKA_OIDC_PROVIDER_URL
OpenId connect  client id | string           | Open ID connect client id                                    | -      | --oidc-client-id  | oidc_client_id  | ANKA_OIDC_CLIENT_ID
OpenId connect  username claim| string           | Open ID connect claim key to use for user name | name      | --oidc-username-claim | oidc_username_claim | ANKA_OIDC_USERNAME_CLAIM
OpenId connect groups claim| string           | Open ID connect claim key to use for groups, | groups      | --oidc-groups-claim | oidc_groups_claim | ANKA_OIDC_GROUPS_CLAIM

### Separate queue interface

 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
queue addr     | string           | address to listen on for queue connections                   | default      | --queue-addr      | queue_addr      | ANKA_QUEUE_ADDR
queue ca cert  | string           | CA cert for queue server                                     | default      | --queue-ca-cert   | queue_ca_cert   | ANKA_QUEUE_CA_CERT
queue server cert| string           | queue server certificate file in PEM format                  | default      | --queue-server-cert | queue_server_cert | ANKA_QUEUE_SERVER_CERT
queue server key| string           | queue server private key file in PEM format                  | default      | --queue-server-key | queue_server_key | ANKA_QUEUE_SERVER_KEY
use queue tls  |                  | enables queue tls                                            | default      | --use-queue-tls   | use_queue_tls   | ANKA_USE_QUEUE_TLS
enable queue auth|                  | enables queue tls                                            | default      | --enable-queue-auth | enable_queue_auth | ANKA_ENABLE_QUEUE_AUTH


### Internal 

Parameters used internally. It's recommended to use the default values.

 Name       | Type        |   Description               | default value | command line | ini    | env 
 ---        |   :---:       | ---                         | :---:           | :---:          | :---:    | :---:
Clean process interval| duration   | The interval to clean the queues (delete any tasks older than 24 hours), 0 to disable (default 1h0m0s) | default      | --clean-queues-interval | clean_queues_interval | ANKA_CLEAN_QUEUES_INTERVAL
allow cors     |                  | If true adds Acces-Control-Allow-Origin to all routes        | default      | --allow-cors      | allow_cors      | ANKA_ALLOW_CORS
scheduler interval| duration         | Interval for checking scheduled tasks       | 30m      | --scheduler-interval | scheduler_interval | ANKA_SCHEDULER_INTERVAL
allowUnknownFlags|                  | Don't terminate the app if ini file contains unknown flags.  | default      | --allowUnknownFlags | allowUnknownFlags | ANKA_ALLOWUNKNOWNFLAGS
dumpflags      |                  | Dumps values for all flags defined in the app into stdout in ini-compatible syntax and terminates the app. | default      | --dumpflags       | dumpflags       | ANKA_DUMPFLAGS 
