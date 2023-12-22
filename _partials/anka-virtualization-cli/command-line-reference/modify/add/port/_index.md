```shell
> anka modify failed add port --help
usage: port-forwarding,port [options] name [rule]

   Add port forwarding rule

arguments:
  name                     Rule name
  rule                     Port forwarding rule: guest-port[:host-ip][:host-port]

options:
  -g,--guest-port <val>    The port inside of the VM that the host-port connects to
  -p,--host-port <val>     The host port to listen on (assigns dynamically if not specified)
  -l,--host-ip <val>       Listen address (defaults to any)
  -d,--delete              Delete the rule
  --set-name <val>         Rename the rule
```
