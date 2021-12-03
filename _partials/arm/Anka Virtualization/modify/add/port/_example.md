```shell
❯ anka modify 12.0.1 add port --guest-port 22 ssh

❯ anka show 12.0.1 network
+------------+------------+
| mode       | shared     |
+------------+------------+
| controller | virtio-net |
+------------+------------+

port_forwarding_rules:
+------+----------+------------+
| name | protocol | guest_port |
+------+----------+------------+
| SSH  | tcp      | 22         |
+------+----------+------------+

❯ anka start 12.0.1

❯ anka show 12.0.1 network
+------------+-------------------+
| mode       | shared            |
+------------+-------------------+
| controller | virtio-net        |
+------------+-------------------+
| mac        | 52:74:36:66:42:d7 |
+------------+-------------------+

port_forwarding_rules:
+------+----------+---------+------------+-----------+
| name | protocol | host_ip | guest_port | host_port |
+------+----------+---------+------------+-----------+
| SSH  | tcp      | 0.0.0.0 | 22         | 10000     |
+------+----------+---------+------------+-----------+

❯ ssh anka@localhost -p 10000
Password:
Last login: Mon Apr  6 12:45:50 2020
Mac-mini:~ anka
```
