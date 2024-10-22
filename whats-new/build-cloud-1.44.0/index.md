---
date: 2024-09-10T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.44.0"
---

### Restart a running VM endpoint

```
curl "http://localhost/api/v1/rpc/vm/restart" -d '{"instance_id":"c2af6080-3ae3-4cd3-64e5-2193f31b9aa5", "vnc_password":"1111111111", "usb_device":"iPhone"}'
{"status":"OK","message":""}
```

The `/v1/registry/vm` endpoint will now have additional property `start_date` in the `vminfo`. For example:

```
❯ curl -s "http://localhost/api/v1/vm" | jq                                     
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "external_id": null,
      "instance_id": "c2af6080-3ae3-4cd3-64e5-2193f31b9aa5",
      "name": null,
      "vm": {
        "instance_id": "c2af6080-3ae3-4cd3-64e5-2193f31b9aa5",
        "instance_state": "Started",
        "message": "Unexpected VM state: \"stopped\"",
        "anka_registry": "http://localhost:8089",
        "vmid": "a4c52e39-5ecf-498f-97fa-bc03acf0e9f9",
        "tag": "ports",
        "vminfo": {
          "uuid": "0e42b576-4d36-482b-8f57-73c005fe8101",
          "name": "mgmtManaged-sonoma-1728740453231034000",
          "status": "running",
          "version": "",
          "creation_date": "2024-10-08T07:57:41Z",
          "access_date": "2024-10-13T11:10:02Z",
          "start_date": "2024-10-13T10:51:34Z",
          "cpu_cores": 4,
          "ram": "4G",
          "node_id": "72650d46-8ebe-4547-bb6c-8eb76e957b0a",
          "host_ip": "192.168.50.70",
          "ip": "192.168.64.25",
          "vnc_port": 5900,
          "port_forwarding": [
            {
              "guest_port": 22,
              "host_port": 22,
              "protocol": "tcp",
              "name": "ssh"
            },
            {
              "guest_port": 5900,
              "host_port": 5900,
              "protocol": "tcp",
              "name": "vnc"
            }
          ],
          "stop_date": "0001-01-01T00:00:00Z",
          "timestamp": "2024-10-13T13:10:02.690025+02:00"
        },
        "node_id": "72650d46-8ebe-4547-bb6c-8eb76e957b0a",
        "inflight_reqid": "7f620141-2be3-46fa-5f7e-16df3958c7d9",
        "ts": "2024-10-13T13:10:07.604294+02:00",
        "cr_time": "2024-10-12T15:40:42.858477+02:00",
        "progress": 0,
        "arch": "arm64",
        "vlan": ""
      }
    }
  ]
}
```


Note that this data is sent by an agent to the controller every `status-heartbeat-time` (5s). Therefore, you will need to to poll `/v1/registry/vm` and check:

1. `"instance_state": "Started"`
2. `"status": "running"`
3. `"start_date"` before restart and `"start_date"` after restart are NOT equal.
