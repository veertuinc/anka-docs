```shell
❯ anka modify 12.6 port --help
usage: port [options] name [rule]

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

❯ anka modify 12.6 port test 5900:0.0.0.0:50000

❯ anka show 12.6 network
. . .
port_forwarding_rules:
+------+----------+------------+-----------+
| name | protocol | guest_port | host_port |
+------+----------+------------+-----------+
| test | tcp      | 22         | 50022     |
+------+----------+------------+-----------+

❯ anka start 12.6

❯ ssh anka@localhost -p 50022
(anka@localhost) Password:
Last login: Fri Oct 14 06:37:54 2022
anka@Ankas-Virtual-Machine ~ % 
```
