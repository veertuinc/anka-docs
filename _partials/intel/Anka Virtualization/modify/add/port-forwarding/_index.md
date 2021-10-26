```shell
> sudo anka modify 11.6.0 add port-forwarding --help
Usage: anka modify add port-forwarding [OPTIONS] RULE_NAME

Options:
  -p, --host-port INTEGER   The host port to listen on
  -g, --guest-port INTEGER  The port inside of the VM that the host-port connects to  [required]
  -l, --host-ip TEXT        The IP to bind the host port with (defaults to 0.0.0.0)
  --guest-ip TEXT           The IP to bind to inside of the VM
  --help                    Display usage information
```
