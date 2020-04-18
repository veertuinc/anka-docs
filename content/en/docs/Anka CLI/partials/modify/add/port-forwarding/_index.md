```shell
> sudo anka modify 10.15.4 add port-forwarding --help
Usage: anka modify add port-forwarding [OPTIONS] RULE_NAME

Options:
  -p, --host-port INTEGER   the port to listen on the host
  -g, --guest-port INTEGER  the port to forward to on the vm  [required]
  -l, --host-ip TEXT        bind forwarding port to address, defaults to 0.0.0.0 (all)
  --guest-ip TEXT           specifies guest IP, in case the guest has multiple IPs
  --help                    Show this message and exit.
```
