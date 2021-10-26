```shell
❯ anka show 12.0-beta
+---------+--------------------------------------+
| uuid    | 26c18e20-f67a-4387-a7b7-236a277bb424 |
+---------+--------------------------------------+
| name    | 12.0-beta (v2)                       |
+---------+--------------------------------------+
| created | Oct 15 08:20:40 2021                 |
+---------+--------------------------------------+
| vcpu    | 8                                    |
+---------+--------------------------------------+
| memory  | 12G                                  |
+---------+--------------------------------------+
| display | 1024x768                             |
+---------+--------------------------------------+
| disk    | 128GiB (22.83GiB on disk)            |
+---------+--------------------------------------+
| addons  | 3.0.0.135.8400565                    |
+---------+--------------------------------------+
| network | shared                               |
+---------+--------------------------------------+
| status  | stopped Oct 18 18:57:50 2021         |
+---------+--------------------------------------+

❯ anka --machine-readable show 12.0-beta | jq
{
  "status": "OK",
  "body": {
    "uuid": "26c18e20-f67a-4387-a7b7-236a277bb424",
    "name": "12.0-beta",
    "version": "v2",
    "creation_date": "2021-10-16T07:20:40Z",
    "cpu_cores": 8,
    "cpu_frequency": 0,
    "cpu_htt": false,
    "ram": "12884901888",
    "ram_size": 12884901888,
    "frame_buffers": 1,
    "hard_drive": 137438953472,
    "encrypted": false,
    "image_size": 24518402048,
    "addons_version": "3.0.0.135.8400565",
    "stop_date": "2021-10-19T17:57:50Z",
    "status": "stopped"
  }
}

❯ anka --machine-readable show 12.0-beta --tag v2 | jq
{
  "status": "OK",
  "body": {
    "uuid": "26c18e20-f67a-4387-a7b7-236a277bb424",
    "name": "12.0-beta",
    "creation_date": "2021-10-19T12:57:57Z",
    "cpu_cores": 8,
    "cpu_frequency": 0,
    "cpu_htt": false,
    "ram": "12884901888",
    "ram_size": 12884901888,
    "frame_buffers": 1,
    "hard_drive": 137438953472,
    "encrypted": false,
    "image_size": 21119369216,
    "addons_version": "3.0.0.135.8400565",
    "stop_date": "2021-10-19T17:58:31Z",
    "status": "stopped"
  }
}
```