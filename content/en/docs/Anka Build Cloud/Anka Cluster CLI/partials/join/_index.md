```shell
> sudo ankacluster join --help
Joins the current machine to Anka Cloud cluster

Usage:
  ankacluster join [controller_address] [flags]

Flags:
  -c, --cacert PATH              PATH to CA bundle file (PEM/X509) (optional)
  -M, --capacity-mode string     Capacity mode (resource|number). 'resource' will take vm jobs according to resources, number will take vm jobs according to max-vm-count (default "number")
  -C, --cert PATH                PATH client certificate file (PEM/X509) (optional)
  -K, --cert-key PATH            PATH client certificate key file (PEM/X509) (optional)
  -f, --force-no-sudo            Force the agent to start without sudo
  -g, --global                   DEPRECATED! Install agent into system domain (optional)
  -G, --groups string            Specify group name (or names sepearated by ',') to add this node to (optional)
      --heartbeat duration       Override the duration between updates
  -h, --help                     help for join
  -H, --host string              Specify host name or IP for this machine (optional)
  -k, --keystore PATH            PATH to certificate and keystore (PEM, PKCS12) (optional)
  -p, --keystore-pass PASSWORD   PASSWORD for certificate and keystore (optional)
  -m, --max-vm-count NUMBER      Maximum NUMBER of VMs this node is allowed to run (optional) (default 2)
  -n, --name NAME                Node NAME alias (optional)
      --no-central-logging       Dont send logs to central logging (optional)
  -R, --ram-override int         Override the max RAM in GB that this node can handle. defaults to (total ram - 2GB)
      --reserve-space string     Disk space to reserve when pulling. Number followed by magnitude (1024B, 10KB, 140MB, 45GB...)
  -r, --root-cert PATH           PATH to CA bundle file (PEM/X509) (optional)
      --skip-tests               Skip testing the connection before starting the agent (optional)
      --skip-tls-verification    Skip TLS verification (optional)
  -t, --tls                      Use tls for communicating with the queue (optional)
  -V, --vcpu-override int        Override the max vcpu that this node can handle. defaults to (cpu count * 2)
```
