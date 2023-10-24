
---
---

{{< hint warning >}}
Be sure to run ankacluster as root.
{{< /hint >}}

{{< hint warning >}}
Avoid using underscores in your domainnames/urls.
{{< /hint >}}

```shell
❯ sudo ankacluster join http://<ip>
Password:
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success
```

- Replace `<ip>` with the IP of the machine hosting your controller:
- If you changed the default port for the controller from 80, you'll need to use the new port at the end of the IP. Otherwise, leave it off.

The command may hang for a few moments and then display `Cluster join success`. Please report any errors you find to support@veertu.com.

{{< hint info >}}
The Anka agent is listening on a socket to provide status information at runtime.
You can override the path of the socket by setting the `ANKA_AGENT_SOCKET` env var.
{{< /hint >}}

``` bash
❯ sudo ankacluster join --help
Joins the current machine to one or many Anka Build Cloud Controllers

Usage:
  ankacluster join CONTROLLER_ADDRESS[,CONTROLLER_ADDRESS2] [flags]

Flags:
      --api-key-file string                   The (non-base64) API Key file path. Takes precedence over api-key-string
      --api-key-id string                     API Key identifier
      --api-key-string string                 API Key string
  -c, --cacert string                         Specify the path to your Root CA Certificate (PEM/X509)
  -M, --capacity-mode string                  Set the capacity mode (resource or number) the Node will use when calculating if it can start a VM task - 'resource' will look at total unused CPU cores and RAM available - 2GB (see --vcpu-override and --ram-override) & 'number' will only accept if --max-vm-count isn't already met (default 'number') (default "number")
  -C, --cert string                           Specify the path to the Node's certificate file (PEM/X509)
  -K, --cert-key string                       Specify the path to the Node's certificate key file (PEM/X509)
      --cli-start-timeout duration            Timeout for anka start command (default 1m30s)
      --dial-timeout duration                 http dial timeout (default 15s)
      --dump-network-meter                    Dump aggregation of http stats to a file (default true)
      --dump-network-meter-file-name string   Filepath to dump http stats to, dir is log dir (default "http-dump.json")
      --enable-vm-monitor                     Enabled unresponsive VM monitoring. This will throw a failure when the VM becomes unresponsive for longer than the --vm-stuck-timeout
  -f, --force-no-sudo                         Force the anka_agent to start without sudo
  -g, --global                                DEPRECATED! Install agent into system domain
  -G, --groups string                         Specify group name (or multiple names sepearated by ',') to add the current Node to
      --heartbeat duration                    Set the duration between status updates the Node sends to the Anka Cloud Cluster (default: 5 seconds)
  -h, --help                                  help for join
  -H, --host string                           Set the address (IP or Hostname) of the Node that the Anka Cloud Cluster will use when communicating with CI tools/plugins. This is useful when your CI tool cannot connect to the Node's local IP address (the default value of --host), but does have access to an external IP or hostname for it (proxy, load balancer, etc).
      --ignore-arm-vm-limit                   Ignore ARM VM Limit
  -k, --keystore string                       Specify the path to your certificate keystore (PEM/PKCS12)
  -p, --keystore-pass string                  Specify the password for your certificate keystore
      --max-idle-connections-per-host int     mac idle connections per host (default 50)
  -m, --max-vm-count int                      Set the maximum number of VMs this Node is allowed to run (default: 2) (default 2)
  -n, --name string                           Set a custom Node name (default: hostname)
      --no-central-logging                    Disable sending logs to central logging location
      --node-id string                        Custom node id
      --num-http-sample-threshold int         The agent will aggregate http stats after this amount of requests (default 1000)
      --num-workers int                       The number of concurrent worker executing tasks (default 2)
      --optimization-threshold int            Number of times to retry to start VM before starting disk space optimization (default 5)
  -R, --ram-override int                      Override ram limit for resource based capacity (default: {total ram} - 2GB)
      --request-timeout duration              http request timeout (default 1m0s)
      --reserve-space string                  Disk space to reserve when pulling. Number followed by magnitude (1024B, 10KB, 140MB, 45GB...) (default: 20% of disk size)
  -r, --root-cert string                      Identical to --cacert
      --skip-pull-error string                pass no-download-bytes for ignoring pull errors when download bytes is 0
      --skip-tests                            Disable testing the connection before starting the agent
      --skip-tls-verification                 Skip TLS verification
  -t, --tls                                   Enable TLS for communicating with the Anka Cloud Cluster
      --tls-handshake-timeout duration        tls handshake timeout (default 5s)
  -V, --vcpu-override int                     Override vcpu limit for resource based capacity (default: {current physical(intel)/performance(arm) cpu count} * 2)
      --verbosity string                      verbosity level, 0 - 10 (higher number - more chatty)
      --vm-stuck-delay duration               The time between unresponsive VM checks (default: 30s - Duration examples: 3500s, 20m, 5h)
      --vm-stuck-timeout duration             The time to wait until the VM is considered unresponsive (default: 10s - Duration examples: 3500s, 20m, 5h)
```

### Check Join Status

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