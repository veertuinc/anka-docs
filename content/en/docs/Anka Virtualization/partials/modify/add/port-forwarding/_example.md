```shell
❯ sudo anka modify 10.15.4 add port-forwarding --guest-port 22 ssh

❯ sudo anka describe 10.15.4

. . .

port_forwarding_rules

+------------+--------------+-------------+------------+-----------+-------------+
|   net_card |   guest_port | rule_name   | protocol   |   host_ip |   host_port |
+============+==============+=============+============+===========+=============+
|          0 |           22 | ssh         | tcp        |         0 |           0 |
+------------+--------------+-------------+------------+-----------+-------------+

❯ sudo anka start 10.15.4
. . . 
port_forwarding

+--------------+-------------+------------+--------+-----------+
|   guest_port |   host_port | protocol   | name   | host_ip   |
+==============+=============+============+========+===========+
|           22 |       10000 | tcp        | ssh    | 0.0.0.0   |
+--------------+-------------+------------+--------+-----------+

❯ ssh anka@localhost -p 10000
Password:
Last login: Mon Apr  6 12:45:50 2020
Mac-mini:~ anka
```
