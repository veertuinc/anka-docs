```shell
> anka modify 26.4.1-arm64 port --help
usage: port [options] name [rule]

   Add port forwarding rule

arguments:
  name                     Rule name
  rule                     Port forwarding rule in form guest-port[:addr][:port]

options:
  -r,--reverse             Proxy from guest to an external address, e.g rule /var/run/syslog:/var/run/syslog
  -d,--delete              Delete the rule
  --set-name <val>         Rename the rule
```
