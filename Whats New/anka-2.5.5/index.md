---
date: 2022-03-29T01:00:00-00:00
title: "Anka Virtualization 2.5.5"
---

### `anka show` now displays cpu usage from inside of the VM

```bash
❯ anka show test
+---------+------------------------------------------+
| uuid    | 80f821a2-2a70-4c42-a487-44598736e19f     |
+---------+------------------------------------------+
| name    | test                                     |
+---------+------------------------------------------+
| created | Mar 25 12:20:05 2022                     |
+---------+------------------------------------------+
| vcpu    | 2 - sys:28.9%, usr:46.3%, idle:24.9%     |
+---------+------------------------------------------+
| memory  | 4G                                       |
+---------+------------------------------------------+
| display | 1024x768 vnc://:admin@192.168.0.103:5901 |
+---------+------------------------------------------+
| disk    | 40GiB (18.38GiB on disk)                 |
+---------+------------------------------------------+
| addons  | 2.5.5.139.120038                         |
+---------+------------------------------------------+
| network | shared 192.168.64.34                     |
+---------+------------------------------------------+
| status  | running since Mar 29 11:11:01 2022       |
+---------+------------------------------------------+
```

```bash
❯ anka --machine-readable show test | jq
{
  "status": "OK",
  "body": {
    "uuid": "80f821a2-2a70-4c42-a487-44598736e19f",
    "name": "test",
    "creation_date": "2022-03-25T11:20:05Z",
    "cpu_cores": 2,
    "cpu_frequency": 0,
    "cpu_htt": false,
    "cpu_user": 372,
    "cpu_system": 264,
    "cpu_idle": 365,
    "cpu_nice": 0,
    "ram": "4G",
    "ram_size": 4294967296,
    "frame_buffers": 1,
    "hard_drive": 42949672960,
    "encrypted": false,
    "image_size": 19835912192,
    "addons_version": "2.5.5.139.120038",
    "vnc_port": 5901,
    "vnc_password": "admin",
    "vnc_connection_string": " vnc://192.168.0.103:5901",
    "status": "running",
    "usb": [],
    "pid": 60516,
    "ip": "192.168.64.34",
    "mac": "36:ab:1d:ff:fd:16",
    "hostif": "vmenet0",
    "port_forwarding": [],
    "time_sync": 0,
    "displays": [
      {
        "width": 1024,
        "height": 768,
        "dpi": 72,
        "location": 105553150495524
      }
    ],
    "start_date": "2022-03-29T10:11:01Z",
    "access_date": "2022-03-29T10:12:19Z"
  }
}
```

The logs also include CPU information for heavy usage and also at the time of suspend.

```bash
❯ cat ~/Library/Logs/Anka/80f821a2-2a70-4c42-a487-44598736e19f.log
2022-03-29 11:11:01: test (pid 60516): session restored
vmx_set_ctlreg: cap_field: 2 bit: 14 must be zero
shadow vmcs disabled!
Error return from kevent change: No such file or directory
Error return from kevent change: No such file or directory
2022-03-29 11:11:17: high CPU load detected: 100%
2022-03-29 11:11:27: high CPU load detected: 99%
2022-03-29 11:13:12: high CPU load detected: 100%
2022-03-29 11:13:17: high CPU load detected: 99%
2022-03-29 11:13:22: high CPU load detected: 100%
2022-03-29 11:13:27: high CPU load detected: 100%
2022-03-29 11:13:32: high CPU load detected: 100%
2022-03-29 11:13:37: high CPU load detected: 99%
2022-03-29 11:13:42: high CPU load detected: 99%
2022-03-29 11:13:47: high CPU load detected: 100%
2022-03-29 11:13:52: high CPU load detected: 100%
2022-03-29 11:13:57: high CPU load detected: 100%
2022-03-29 11:14:02: high CPU load detected: 100%
2022-03-29 11:14:07: high CPU load detected: 100%
2022-03-29 11:14:12: high CPU load detected: 99%
2022-03-29 11:14:17: high CPU load detected: 99%
2022-03-29 11:14:22: high CPU load detected: 99%
2022-03-29 11:14:27: high CPU load detected: 100%
2022-03-29 11:18:05: vm: suspended with status 0, cpu load 82%
```

<!-- ### Ability to separate runtime image from static image storage directories (**experimental**)

By default, runtime and static images for VMs are stored in `img_lib_dir`. If the disk performance is lacking for this location, you can change `anka config tag_lib_dir` (and `state_lib_dir`) to target the slower disk and move `img_lib_dir` to a faster disk. Runtime images for VMs (which are modified when a VM is running) are then stored in `img_lib_dir`.

{{< hint warning >}}
Delete all VM templates before making this change and then re-pull them post-change.
{{< /hint >}}

{{< hint warning >}}
Note that VM creation will happen into `img_lib_dir` and might either fill up your disk or use a lot of IO (which can be expensive). We recommend VM creation on a slower machine and then pulling the new VM to the machine where you've configured `tag_lib_dir`.
{{< /hint >}} -->
