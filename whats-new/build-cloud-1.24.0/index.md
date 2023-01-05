---
date: 2022-05-01T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.24.0"
---

### Start VM Instance API `startup_script` monitoring

Starting in 1.24.0, you can now pass in extra parameters for Start VM instance API calls that allow greater control over handling failures in the scripts being run as well as the API response JSON providing information from the script execution instead of just the agent logs.

Parameter | Type   | Description       | Default
---       | ---    |   ---             | ---
script_monitoring | bool | Enable script monitoring. This will put the instance to Error state if exit code is not 0. Enables script_fail_handler and script_timeout parameters. | false
script_timeout | int | Seconds. Will terminate startup script execution and treat it as failed. **(only works when script_monitoring is true)** | 90
script_fail_handler | int | How to handle VM on node when startup script fails. Options are 0, 1 and 2. If 0 is passed, VM will be stopped, if 1 is passed VM will be kept alive, if 2 is passed VM will be deleted **(only works when script_monitoring is true)** | 0

```bash
❯ echo -n "echo 123;" | base64
ZWNobyAxMjM7

❯ curl -X POST "http://anka.controller:8090/api/v1/vm" -H "Content-Type: application/json" \
  -d '{"vmid": "15698722-4220-452b-95c3-be8099117dc1", "count": 1, "startup_script": "ZWNobyAxMjM7", "script_monitoring": true}'

❯ curl  "http://anka.controller:8090/api/v1/vm" -H "Content-Type: application/json" | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "external_id": null,
      "instance_id": "36f4147f-0707-44ee-63c5-deb21bc1849e",
      "name": null,
      "vm": {
        "instance_id": "36f4147f-0707-44ee-63c5-deb21bc1849e",
        "instance_state": "Started",
        "anka_registry": "http://anka.registry:8089",
        "vmid": "15698722-4220-452b-95c3-be8099117dc1",
        "vminfo": {
          "uuid": "f4f8b659-7c6d-425c-8965-8a440d2f1b10",
          "name": "mgmtManaged-12.3.1-openjdk-11.0.14.1-1652386928609427000",
          "cpu_cores": 4,
          "ram": "8G",
          "status": "running",
          "node_id": "3c101836-9540-4733-9482-604d0c5fbe30",
          "host_ip": "192.168.0.103",
          "ip": "192.168.64.64",
          "vnc_port": 5901,
          "vnc_connection_string": "vnc://:admin@192.168.0.103:5901",
          "vnc_password": "admin",
          "port_forwarding": [
            {
              "guest_port": 22,
              "host_port": 10000,
              "protocol": "tcp",
              "rule_name": ""
            }
          ],
          "creation_date": "2022-04-21T16:26:01Z",
          "stop_date": "0001-01-01T00:00:00Z",
          "version": ""
        },
        "node_id": "3c101836-9540-4733-9482-604d0c5fbe30",
        "ts": "2022-05-12T16:22:33.086557-04:00",
        "cr_time": "2022-05-12T16:22:04.287033-04:00",
        "progress": 0,
        "arch": "amd64",
        "vlan": "",
        "startup_script": {
          "return_code": 0,
          "did_timeout": false,
          "stdout": "123\n",
          "stderr": ""
        }
      }
    },
```

{{< hint warning >}}
`vminfo` in the VM JSON will show the status of the VM from `anka show` on the node and if the VM failed and was deleted from the node, it will have a status of `deleted`.
{{< /hint >}}

The agent logs will still display the output too:

```bash
I0512 16:22:07.553012   96837 agent.go:20] starting vm start process, source vm: 15698722-4220-452b-95c3-be8099117dc1
I0512 16:22:08.609453   96837 agent.go:173] cloning 15698722-4220-452b-95c3-be8099117dc1 with name mgmtManaged-12.3.1-openjdk-11.0.14.1-1652386928609427000
I0512 16:22:10.223570   96837 agent.go:76] Running script on VM f4f8b659-7c6d-425c-8965-8a440d2f1b10
W0512 16:22:32.088549   96837 probes.go:270] Filler node-name run waited more than 20s. last run: 2022-05-12 16:22:10.580581 -0400 EDT m=+2417.079147039 it was 21.507298647s ago
I0512 16:22:32.259533   96837 agent.go:143] Script exit code: 0
I0512 16:22:32.259566   96837 agent.go:144] Script stdout: 123
I0512 16:22:32.259573   96837 agent.go:145] Script stderr:
I0512 16:22:32.259583   96837 start_vm.go:190] returned from startvmRequest
I0512 16:22:32.259592   96837 start_vm.go:191] requestId:f6debd4d-9aa4-4799-447c-4914b937d930, succeeded . instance id36f4147f-0707-44ee-63c5-deb21bc1849e
```

### 