```shell
> anka modify 12.3.1 port --help
usage: port [options] name

   Add port forwarding rule

arguments:
  name                     Rule name

options:
  -g,--guest-port <val>    The port inside of the VM that the host-port connects to
  -p,--host-port <val>     The host port to listen on (assigns dynamically if not specified)
  -l,--host-ip <val>       The IP to bind the host port with (defaults to any)
  -d,--delete              Delete the rule
  --set-name <val>         Rename the rule
```
