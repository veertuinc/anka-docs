```shell
> sudo ankacluster test --help
Tests connection from this machine the cluster

Usage:
  ankacluster test [controller_address] [flags]

Flags:
  -c, --cacert PATH              PATH to CA bundle file (PEM/X509) (optional)
  -C, --cert PATH                PATH client certificate file (PEM/X509) (optional)
  -K, --cert-key PATH            PATH client certificate key file (PEM/X509) (optional)
  -g, --global                   Install agent into system domain (optional)
  -G, --groups string            Specify group name (or names sepearated by ',') to add this node to (optional)
  -h, --help                     help for test
  -k, --keystore PATH            PATH to certificate and keystore (PEM, PKCS12) (optional)
  -p, --keystore-pass PASSWORD   PASSWORD for certificate and keystore (optional)
  -m, --max-vm-count NUMBER      Maximum NUMBER of VMs this node is allowed to run (optional) (default 2)
  -n, --name NAME                Node NAME alias (optional)
  -r, --root-cert PATH           PATH to CA bundle file (PEM/X509) (optional)
      --skip-tls-verification    Skip TLS verification (optional)
  -t, --tls                      Use tls for communicating with the queue (optional)
```
