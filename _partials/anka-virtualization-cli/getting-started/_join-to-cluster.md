
---
---

{{< hint warning >}}
Be sure to run ankacluster as root.
{{< /hint >}}

{{< hint warning >}}
Avoid using underscores in your domainnames/urls.
{{< /hint >}}

```shell
❯ sudo ankacluster join http://<ip>
Password:
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success
```

- Replace `<ip>` with the IP of the machine hosting your controller:
- If you changed the default port for the controller from 80, you'll need to use the new port at the end of the IP. Otherwise, leave it off.

The command may hang for a few moments and then display `Cluster join success`. Please report any errors you find to support@veertu.com.

{{< hint info >}}
The Anka agent is listening on a socket to provide status information at runtime.
You can override the path of the socket by setting the `ANKA_AGENT_SOCKET` env var.
{{< /hint >}}

### Check Join Status

You can check the status of the Anka agent using the `ankacluster status` command.


```shell
❯ ankacluster status
status: running
config:
  vm_limit: 2
  optimization_threshold: 5
  num_workers: 2
  controller_addresses:
  - http://anka.controller.dev
  version: 1.13.0-6cd34a2c
  capacity_mode: number
  heartbeat: 5s
  node_name: MyMacMiniNode
  vm_stuck_check_delay: 30s
  vm_stuck_check_timeout: 10s
```