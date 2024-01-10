---
---

{{< hint info >}}
We do not recommend setting `--host-port`, as it will cause collisions if two VMs attempt to use the same port. If no `--host-port` is specified, we will dynamically assign them starting from 10000 and incrementing (10001, 10002, etc) depending on if there are other VMs running and consuming ports from the range already.
{{< /hint >}}

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

❯ anka modify 12.6 port ssh 22:0.0.0.0

❯ anka start 12.6

❯ anka show 12.6 network
. . .
port_forwarding_rules:
+------+----------+------------+-----------+
| name | protocol | guest_port | host_port |
+------+----------+------------+-----------+
| ssh | tcp      | 22         | 10000     |
+------+----------+------------+-----------+

❯ ssh anka@localhost -p 10000
(anka@localhost) Password:
Last login: Fri Oct 14 06:37:54 2022
anka@Ankas-Virtual-Machine ~ % 
```
