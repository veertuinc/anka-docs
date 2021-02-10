---
date: 2019-12-12T00:00:00-00:00
title: "Joining your Nodes to the Controller"
linkTitle: "Joining your Nodes to the Controller"
weight: 4
description: >
  How to join your Anka Build Virtualization Nodes to the Anka Build Cloud Controller
---

## Prerequisites

* [Anka Build Cloud Controller should be configured and running before your Anka Node can join]({{< relref "docs/Anka Build Cloud/_index.md" >}})
* [Your Node should be licensed]({{< relref "docs/Licensing/_index.md" >}})
* [Your Node should be prepared for high availability]({{< relref "docs/Anka Build Cloud/prepare-nodes.md" >}})
## Joining to your Anka Build Cloud Controller

> Be sure to run ankacluster as root

> **DO NOT use underscores in your CNAME/URLs**

```shell
❯ sudo ankacluster join http://anka.controller:8090
Testing connection to the controller...: Ok
Testing connection to the registry...: Ok
Success!
Anka Cloud Cluster join success
```

> You can join a Node to multiple controllers by comma separating them:
> `sudo ankacluster join http://anka.controller1:8090,http://anka.controller2:8090`

```shell
❯ ankacluster join --help
Joins the current Node to your Anka Cloud Cluster

Usage:
  ankacluster join [controller_address] [flags]

Flags:
  -c, --cacert string               Specify the path to your Root CA Certificate (PEM/X509)
  -M, --capacity-mode string        Set the capacity mode (resource or number) the Node will use when pulling jobs from the Anka Cloud Cluster queue. 'resource' will look at available resources (see --vcp-override and --ram-override) / 'number' will only accept if --max-vm-count isn't already met (default "number")
  -C, --cert string                 Specify the path to the Node's certificate file (PEM/X509)
  -K, --cert-key string             Specify the path to the Node's certificate key file (PEM/X509)
      --enable-vm-monitor           Enabled unresponsive VM monitoring. This will throw a failure when the VM becomes unresponsive for longer than the --vm-stuck-timeout
  -f, --force-no-sudo               Force the anka_agent to start without sudo
  -g, --global                      DEPRECATED! Install agent into system domain
  -G, --groups string               Specify group name (or multiple names sepearated by ',') to add the current Node to
      --heartbeat duration          Set the duration between status updates the Node sends to the Anka Cloud Cluster (default: 5 seconds)
  -h, --help                        help for join
  -H, --host string                 Set the address (IP or Hostname) of the Node that the Anka Cloud Cluster will use when communicating with CI tools/plugins. This is useful when your CI tool cannot connect to the Node's local IP address (the default value of --host), but does have access to an external IP or hostname for it (proxy, load balancer, etc).
  -k, --keystore string             Specify the path to your certificate keystore (PEM/PKCS12)
  -p, --keystore-pass string        Specify the password for your certificate keystore
  -m, --max-vm-count int            Set the maximum number of VMs this Node is allowed to run (default: 2) (default 2)
  -n, --name string                 Set a custom Node name (default: hostname)
      --no-central-logging          Disable sending logs to central logging location
  -R, --ram-override int            Set the the max RAM (in GB) that this Node can handle (default: {total ram} - 2GB)
      --reserve-space string        Set the space the Node will reserve when it receives a job and pulls the Template/Tag. This is useful if you want to ensure there is always enough space for other application/disk usage. (Format: 1024B, 10KB, 140MB, 45GB, etc)
  -r, --root-cert string            Identical to --cacert
      --skip-tests                  Disable testing the connection before starting the agent
      --skip-tls-verification       Skip TLS verification
  -t, --tls                         Enable TLS for communicating with the Anka Cloud Cluster
  -V, --vcpu-override int           Set the max vcpus that this Node can handle. (default: {current physical cpu count} * 2)
      --vm-stuck-delay duration     The time between unresponsive VM checks (default: 30s - Duration examples: 3500s, 20m, 5h)
      --vm-stuck-timeout duration   The time to wait until the VM is considered unresponsive (default: 10s - Duration examples: 3500s, 20m, 5h)
  ```

The Anka agent is listening on a socket to provide status information at runtime.
You can override the path of the socket by setting the `ANKA_AGENT_SOCKET` env var.

## Disjoining

> You don't need to disjoin nodes to upgrade the Anka Virtualization package

```shell
❯ sudo ankacluster disjoin
Disjoined from the Anka Cloud Cluster
```

## Status

You can check the status of the Anka agent using the `ankacluster status` command.


```shell
❯ ankacluster status
status: running
config:
  vm_limit: 2
  optimization_threshold: 5
  num_workers: 2
  controller_addresses:
  - http://anka.controller.dev
  version: 1.13.0-6cd34a2c
  capacity_mode: number
  heartbeat: 5s
  node_name: MyMacMiniNode
  vm_stuck_check_delay: 30s
  vm_stuck_check_timeout: 10s
```

For infomation check out `ankacluster status --help`

```shell
❯ sudo ankacluster status --help
Returns the status of the Anka Node Agent

Usage:
  ankacluster status [flags]

Flags:
  -h, --help               help for status
  -m, --machine-readable   Output the response in json format
```


The Anka agent is listening on a socket to provide the information at runtime.
In case the agent is configured to listen on a custom socket, you can use the `ANKA_AGENT_SOCKET` env var to override it.
