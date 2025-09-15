---
date: 2025-09-15T01:00:00-00:00
title: "Anka Virtualization 3.8.0"
---

## Labels for the VM Template/Tag are now available inside of the VM

```bash
❯ anka modify anklet label labelA testA
❯ anka start anklet
+----------------+--------------------------------------+
| uuid           | 84266873-da90-4e0d-903b-ed0233471f9f |
+----------------+--------------------------------------+
| name           | anklet                               |
+----------------+--------------------------------------+
| created        | Aug 27 09:47:04 2025                 |
+----------------+--------------------------------------+
| vcpu           | 6                                    |
+----------------+--------------------------------------+
| ram            | 14G                                  |
+----------------+--------------------------------------+
| display        | 1024x768                             |
+----------------+--------------------------------------+
| disk           | 100GiB (62.83GiB on disk)            |
+----------------+--------------------------------------+
| addons_version | 3.7.2.203                            |
+----------------+--------------------------------------+
| network        | shared                               |
+----------------+--------------------------------------+
| status         | running since Sep 15 16:13:13 2025   |
+----------------+--------------------------------------+
❯ anka run anklet bash -c "nc -U /var/run/anka"
uuid: 84266873-da90-4e0d-903b-ed0233471f9f
license: com.veertu.anka.entplus,h:255
name: anklet
labelA: testA
version: 3.8.0
```

## Configurable IO buffer size for `anka push`

There are now two new config options for `anka push` to control the IO buffer size. The default is `0` which means the default buffer for curl is used.

```bash
❯ anka config send_buffer_size
0
❯ anka config recv_buffer_size
0
```

## New `anka log` command

```bash
❯ anka log --help
usage: log [options] <command>

   Show and manage the logs

options:
  -s,--since <val>         Handle logs since the date provided (last hour by default)
  -t,--till <val>          Handle logs before the date provided
  -a,--all                 Handle all logs
  -l,--level <val>         Show logs of the level provided and above (warning by default)

commands:
  clean                    Clean old log files (1 year if the --till parameter isn't specified)
```