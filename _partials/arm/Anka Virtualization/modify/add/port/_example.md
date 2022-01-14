```shell
❯ anka modify 12.1.0-arm add port --guest-port 22 ssh

❯ anka show 12.1.0-arm network           
+------------+------------+
| mode       | shared     |
+------------+------------+
| controller | virtio-net |
+------------+------------+

port_forwarding_rules:
+------+----------+------------+
| name | protocol | guest_port |
+------+----------+------------+
| ssh  | tcp      | 22         |
+------+----------+------------+

❯ anka start 12.1.0-arm

❯ anka show 12.1.0-arm network
+------------+-------------------+
| mode       | shared            |
+------------+-------------------+
| controller | virtio-net        |
+------------+-------------------+
| mac        | ce:73:f0:49:b1:3d |
+------------+-------------------+

port_forwarding_rules:
+------+----------+---------+------------+-----------+
| name | protocol | host_ip | guest_port | host_port |
+------+----------+---------+------------+-----------+
| ssh  | tcp      | 0.0.0.0 | 22         | 10000     |
+------+----------+---------+------------+-----------+

❯ ssh anka@localhost -p 10000
Password:
Last login: Fri Jan 14 17:46:28 2022
anka@12-1-0-arm ~ % 
```
