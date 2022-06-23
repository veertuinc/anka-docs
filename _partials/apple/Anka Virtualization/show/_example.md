```shell
❯ anka show 12.2.0-arm                   
+---------+--------------------------------------+
| uuid    | a6f24306-2af7-45ed-9d70-3a3c1ee7f03a |
+---------+--------------------------------------+
| name    | 12.2.0-arm (vanilla)                 |
+---------+--------------------------------------+
| created | Jan 6 12:34:52 2022                  |
+---------+--------------------------------------+
| vcpu    | 4 ARM                                |
+---------+--------------------------------------+
| ram     | 4G                                   |
+---------+--------------------------------------+
| display | 1024x768                             |
+---------+--------------------------------------+
| disk    | 128GiB (17.72GiB on disk)            |
+---------+--------------------------------------+
| addons  | 3.0.0.135.13568747                   |
+---------+--------------------------------------+
| network | shared                               |
+---------+--------------------------------------+
| status  | stopped Jan 6 18:10:17 2022          |
+---------+--------------------------------------+

❯ anka --machine-readable show 12.2.0-arm | jq
{
  "status": "OK",
  "body": {
    "uuid": "a6f24306-2af7-45ed-9d70-3a3c1ee7f03a",
    "name": "12.2.0-arm",
    "version": "vanilla",
    "creation_date": "2022-01-06T12:34:52Z",
    "cpu_cores": 4,
    "cpu_frequency": 0,
    "cpu_htt": false,
    "arch": 2,
    "ram": "4294967296",
    "ram_size": 4294967296,
    "frame_buffers": 1,
    "hard_drive": 137438953472,
    "encrypted": false,
    "image_size": 19035295744,
    "addons_version": "3.0.0.135.13568747",
    "stop_date": "2022-01-06T18:10:17Z",
    "status": "stopped"
  }
}

❯ anka --machine-readable show 12.2.0-arm --tag v2 | jq                     
{
  "status": "OK",
  "body": {
    "uuid": "a6f24306-2af7-45ed-9d70-3a3c1ee7f03a",
    "name": "12.2.0-arm",
    "version": "v2",
    "creation_date": "2022-01-14T12:18:31Z",
    "cpu_cores": 4,
    "cpu_frequency": 0,
    "cpu_htt": false,
    "arch": 2,
    "ram": "4294967296",
    "ram_size": 4294967296,
    "frame_buffers": 1,
    "hard_drive": 137438953472,
    "encrypted": false,
    "image_size": 19528679424,
    "addons_version": "3.0.0.135.13568747",
    "stop_date": "2022-01-14T17:19:08Z",
    "status": "stopped"
  }
}
```