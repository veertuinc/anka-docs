---
date: 2019-12-12T00:00:00-00:00
title: "Working with the Controller and API"
linkTitle: "Working with the Controller and API"
weight: 4
description: >
  How to work with the Anka Controller and API
---

Anka Controller is the central management system of Anka Build and provides a simple and extensible interface for provisioning and managing on-demand macOS VMs on a cluster of mac hardware (Anka Build nodes).
If you use CI tools like Jenkins, Teamcity. GitLab CI, BuidKite, integrate them with Anka controller using plugins or controller REST APIs to provision macOS VMs on-demand for CI job requests.  

You can work with controller through the web portal interface, REST APIs or the CI plugins.

---

## Controller Portal

Access it by going to your controller IP - `http://<controllerIP>:port`.

You can view the status of your Anka Build macOS cloud from this UI and also perform basic management operations.  

### Dashboard View

This view displays the total active build nodes, running VM instances, instance run capacity utilization, registry storage consumption, average cpu and ram utilization across the entire cloud.

![image1]({{< siteurl >}}images/using-controller/image1.png)

### Nodes View

Click on nodes to go to node list view.

You can view all active build nodes, instances running on them, their cpu and ram utilization. From this view, you can modify the concurrent VM capacity for each node.

![image2]({{< siteurl >}}images/using-controller/image2.png)

#### Change Node Configuration

##### Drain Mode

{{< include file="_partials/anka-build-cloud/whatsnew/1.32.0/drain-mode.md" >}}

#### Explanation of Node States

- **Offline:** Node has not checked in recently
- **Inactive (Invalid License)**: License has likely expired; log in to the node and run `anka license show`
- **Active:** Node is healthy
- **Updating:** Node is downloading and installing the proper agent pkg (if the controller has been upgraded)
- **Unhealthy:** Only set if node's capacity is full AND all of the vms are NOT in "running" state
- **Drain Mode:** Node will not pick up new start VM tasks. Existing VMs will not be impacted

### Templates View

Click on templates to look at all VMs stored in the registry.

![image3]({{< siteurl >}}images/using-controller/image3.png)

Click on an individual template to view all versions/tags for that template.

![image4]({{< siteurl >}}images/using-controller/image4.png)

Click on distribute to all nodes or select specific Nodes to pre-load the most frequently used Vm templates for your build/test jobs on all build nodes. This will reduce the time for first time job execution on Anka Build cloud. Controller manages disk space on the Build Nodes and deletes VM templates that are not used for CI build and test jobs.

> Note - Once a job executes on a build node in a specific VM, the original VM template used for this job is cached on the node. Hence, any subesequent job executions don't download VM from the registry. The VM template is only downloaded when there is a brand new VM template or a new tag to an existing VM template. Download of a VM template with a new tag is only incremental.

![image5]({{< siteurl >}}images/using-controller/image5.png)

Distribution to nodes is complete.

![image6]({{< siteurl >}}images/using-controller/image6.png)

### Instances View

Click on Instances to get a list of all running instances on the cloud.

![image7]({{< siteurl >}}images/using-controller/image7.png)

### Manually starting instances

Click on create instance to manually start instances using a specific VM template/tag on the cloud.

![image8]({{< siteurl >}}images/using-controller/image8.png)

### Accessing Error logs

Starting from Controller release version 1.0.12, logs will be available for download from the Controller Management portal for error scenarios during VM provisioning.

![image9]({{< siteurl >}}images/using-controller/image9.png)

![image10]({{< siteurl >}}images/using-controller/image10.png)

---

### Enterprise License Features

#### Node Groups

{{< include file="_partials/anka-build-cloud/_node_groups.md" >}}

#### Priority Scheduling

{{< include file="_partials/anka-build-cloud/_priority-scheduling.md" >}}

#### USB Device Control (Controller API)

{{< include file="_partials/anka-build-cloud/_usb-device-control-api.md" >}}

#### Event logging and automated pushing

{{< include file="_partials/anka-build-cloud/_event-logging-endpoint-pushing.md" >}}

---

## REST API

#### Health Check

{{< include file="_partials/anka-build-cloud/whatsnew/1.41.1/health-check.md" >}}

#### Status

```bash
❯ curl -s http://anka.controller/api/v1/status         
{"status":"OK","message":"","body":{"status":"Running","version":"1.13.0-24e848a5","registry_address":"http://anka.registry:8089","registry_status":"Running","license":"enterprise plus"}}
```

---

### VM Instance

**Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
instance_id    | string | identifier for the instance
instance_state | string | the instance's state options are "Scheduling", "Pulling", "Started", "Stopping", "Stopped", "Terminating", "Terminated", "Error", "Pushing" (Scheduling -> Pulling, Started, Stopped, Pushing, or Error -> Terminating -> Terminated)
message        | string | Error message in case of an error
anka_registry  | string | the URL of the registry where the template is saved
vmid           | string | The UUID the VM template you wish to target
tag            | string | the template's tag
version        | int    | the template's version
vminfo         | object | an object representing the VM itself
node_id        | string | the Id of the node where the VM is running
inflight_reqid | string | the Id of the pending start VM request
ts             | datetime | time of the instance last update
cr_time        | datetime | creation time of the instance
progress       | float  | the pull progress, in case the VM is in state "Pulling"
group_id       | string | the id of the group that the instance belongs to
name           | string | name of the instance. non unique
external_id    | string | a string saved with the instance, can be used to save the vm id on an external system. non unique
metadata       | object | key-value object. keys are strings. values are strings, integers or booleans
mac_address       | string | represents the assigned MAC address
arch  | string | represents the VM's architecture (amd64 or arm64)
vlan  | string | represnets the VM's VLAN_ID
startup_script | object | [holds information about startup script execution (only if marked monitored)]({{< relref "#startup-script" >}})

{{< hint info >}}
All fields but the following are omitted if empty: `vmid`, `group_id`, `instance_state`, `anka_registry`, `ts`, `cr_time`, `progress`, `vlan` and `usb_device`.
{{< /hint >}}


### VM Info

**Object Model:**

**VM Info Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
uuid           | string | The UUID of the VM
name           | string | The name of the VM
status         | string | The status of the VM
version        | string | The version of the VM
creation_date  | datetime | The creation date of the VM
access_date    | datetime | The last access date of the VM
start_date     | datetime | The start date of the VM
cpu_cores      | int    | The number of CPU cores assigned to the VM
ram            | string | The amount of RAM assigned to the VM
node_id        | string | The ID of the node where the VM is running
host_ip        | string | The host IP address
ip             | string | The IP address of the VM
vnc_port       | int    | The VNC port number
port_forwarding | array of objects | The port forwarding configuration
stop_date      | datetime | The stop date of the VM
timestamp      | datetime | The timestamp of the last update

**Port Forwarding Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
guest_port     | int    | The guest port number
host_port      | int    | The host port number
protocol       | string | The protocol used (e.g., tcp)
name           | string | The name of the port forwarding rule

##### Startup Script

**Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
return_code    | int                | Exit code of the startup script (-1 if timed out)
errors         | array of strings   | Errors if occurred (if none, this field will be omitted)
did_timeout    | bool               | Specifies if the script timed out
stdout         | string             | Stdout of script
stderr         | string             | Stderr of script

#### Start VM instances

{{< hint warning >}}
Group_id, priority and USB_device is only available if you are running Enterprise and higher tier of Anka Build.
{{< /hint >}}

{{< hint info >}}
If you are writing your own SDK to make API calls, be sure to set `external_id` with an identifier for you to troubleshoot with. This helps you tie a VM instance to your custom software visually when investigating issues.
{{< /hint >}}

**Description:** Start VM instances  
**Path:**   /api/v1/vm  
**Method:** POST  
**Required Data Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 vmid      | string | The UUID for the VM Template you wish to use to run a VM.

**Optional Data Parameters:**

Parameter | Type   | Description       | Default
---       | ---    |   ---             | ---
tag       | string | Specify a specific tag to use | Latest tag.
version   | int    | Specify a version number instead of a tag. | -
name      | string | A name for the instance. | -
external_id | string | An arbitrary string to be saved with the instance | -
count     | int    | Number of instances to start. | 1
node_id   | string | Start the instance on this specific node | -
startup_script | string | Script to be executed after the instance is started, encoded as a base64 string. You can set ENVs as well as the script (example: `REPO_URL=repourl TOKEN=123 UUID=abc-123 /Users/admin/startup.sh`). Under the hood, this is performing `anka run {vmname} sh` and passing the decoded base64 into STDIN. You can find the `startup_script` results in the /api/v1/vm results only when `script_monitoring` is set to `true`. | -
startup_script_condition | int | Options are 0 or 1. If 0 is passed the script will run after the VM's network is up, if 1 is passed the script will run as soon as the VM boots. | 0
script_monitoring | bool | Enable script monitoring. This will put the instance to Error state if exit code is not 0. Enables script_fail_handler and script_timeout parameters. | false
script_timeout | int | Seconds. Will terminate startup script execution and treat it as failed. **(only works when script_monitoring is true)** | 90
script_fail_handler | int | How to handle the **VM running on your host/node** when startup script fails. Options are 0, 1 and 2. If 0 is passed, VM will be stopped, if 1 is passed VM will be kept alive, if 2 is passed VM will be deleted **(only works when script_monitoring is true)** | 0
name_template | string | String to use for the VM name. You can interpolate several variables in the string: $template_name, $template_id, $instance_id, $node_id, $node_name and $ts (timestamp). The VM name will be prepended with `mgmtManaged-{name_template}`. | -
group     | string | The Permissions Group name to use when creating the VM. **(REQUIRED and only available when [Resource Permissions for Permission Groups]({{< relref "anka-build-cloud/Advanced Security Features/authorization.md" >}}) is enabled)** | -
group_id  | string | Run the VM on a specific Node Group, targeting its ID **(disabled when [Resource Permissions for Permission Groups]({{< relref "anka-build-cloud/Advanced Security Features/authorization.md" >}}) is enabled)** | -
priority  | int    | Priority of this instance in range 1-10000 (lower is more urgent). | 1000
usb_device | string | Name of the USB device to attach to the VM | -
vcpu      |  int    | Override the number of CPU cores for the VM Template **(only works when the template VM is stopped)**.
vram      |  int    | Override the VM's RAM size in MB (1GB = 1024MB) **(only works when the template VM is stopped)**.
metadata  | object  | Sets the instance metadata, a key-value object. Keys are strings. Values are strings, ints or booleans | -
mac_address      |  string    | Specify MAC address for the VM (Capital letters and ':' as separators) **(only works when the VM Template is stopped and when ANKA_MANAGE_MAC_ADDRESSES is enabled in the controller config)**.
vlan_tag | string | Specify the VLAN ID to target when starting the VM. This will run `anka modify {clonedVMName} set network-card --vlan {ID}` on the host running the VM **and only works when the VM Template is stopped and has `bridge` mode networking**.
video_controller | string | Modify the VM's display controller before starting it. Valid Values: fbuf, pg **(requires Intel and stopped Template)**
hvapic | string | Modify the VM's hvapic value before starting it. Valid Values: forceOn, forceOff **(requires Intel and stopped Template)**
csr_active_config | string | Modify the VM's csr-active-config value before starting it. Valid Values: forceOn **(requires Intel and stopped Template)**
network_local | string | Modify the VM's network locality before starting it. Valid Values: forceOn, forceOff
hw_serial | string | Modify the hw.serial of the VM **(requires Intel and stopped Template)**.
hw_uuid | string | Modify the hw.uuid of the VM **(requires Intel and stopped Template)**.
port_forwarding_override | array[object] | An array of objects, each object being the port forwarding rule to apply. Any pre-existing rules will be ignored and ONLY the ones specified made available.
network_mode | string | Set the network mode for the VM.
disk | object | Set the disk size for the VM. By defautl, it will issue `sudo diskutil apfs resizeContainer` inside of the VM before `startup_script` or the VM is available.
vnc_password | string | Set the VNC password for the VM (currently only for intel). Similar to using `ANKA_VNC_PASSWORD` on the CLI.

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *body:* Array of instance UUIDs  
- *message:* Error message in case of an error

##### Example

```shell
❯ curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
  -d '{"vmid": "6b135004-0c89-43bb-b892-74796b8d266c", "count": 2}'

{
  "status": "OK",
  "message": "",
  "body": [
    "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce",
    "e74dfc0e-dc94-4ca2-575e-3219ac08ffa2"
  ]
}
```

##### Example for `port_forwarding_override`

Each object in the array as the following parameters:

 Parameter | Type   | Description
 ---       |   ---  |          ---
 name       | string | Name of the rule. **Required**
 guest_port | string | Port inside of the VM to make available on host. **Required**
 host_ip    | string | Host IP.
 host_port  | string | Port on host where the VM's port will be available. If not set, defaults to 10000, 10001, etc.

```shell
❯ curl http://anka.controller:8090/api/v1/vm -d '{"vmid":"c12ccfa5-8757-411e-9505-128190e9854e","port_forwarding_override":[{"host_port":"8080", "name":"web","guest_port":"80"},{"name":"vnc","guest_port":"5900"}]}'

{"status":"OK","message":"","body":["4eb107b9-2ce1-4d00-4f6b-d7b03c8b107e"]}
```

##### Example for `disk`

 Parameter | Type   | Description
 ---       |   ---  |          ---
 size       | string | Target size of VM disk in T, G, M, or K. **Required**
 skip_container_resize | boolean | Do not issue the `diskutil` resize command inside of the VM on start.

```shell
❯ curl http://anka.controller:8090/api/v1/vm -d '{"vmid":"c12ccfa5-8757-411e-9505-128190e9854e","disk": {"size": "256G", "skip_container_resize": true}}'
{"status":"OK","message":"","body":["d90c2339-4d9c-42ad-6c8a-81469e7eadbe"]}
```

#### Update Instance

**Description:** Update VM Instance   
**Path:**  /api/v1/vm  
**Method:** PUT   
**Required Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id      | string | Return the VM with that ID. If the vm does not exists the server will return the status `FAIL`

**Optional Data Parameters:**

 Parameter     | Type   | Description              | Default
 ---           | ---    |   ---                    | ---
 name          | string | A name for the instance  | -
 external_id   | string | An arbitrary string to be saved with the instance   | -
 metadata      | object | Updates the instance metadata, a key-value object. Keys are strings. Values are strings, ints or booleans | -

 **Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
 curl -X PUT "http://anka.controller/api/v1/vm?id=c0f36a87-41d9-44a8-66e1-6c5afae15b80" -H "Content-Type: application/json" \
  -d '{"name": "My VM name"}'

{
  "status": "OK",
  "message": ""
}
```

#### Terminate VM instance

**Description:** Terminate a running VM instance  
**Path:** /api/v1/vm  
**Method:** DELETE  
**Required Data Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id      | string | The id of the instance to terminate.

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X DELETE "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
-d '{"id": "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce"}' | jq
{
   "status": "OK",
   "message": ""
}
```

#### Restart VM instance

**Description:** Restart a running VM instance  
**Path:** /api/v1/rpc/vm/restart  
**Method:** POST  
**Required Data Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 instance_id      | string | The id of the instance to restart.

**Optional Data Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 usb_device      | string | The name of the USB device to attach to the VM.
 vnc_password      | string | The VNC password for the VM.

Note that data is sent by an agent to the controller every `status-heartbeat-time` (5s). Therefore, in order to check the status of the restart, you will need to to poll `/v1/registry/vm` and check:

1. `"instance_state": "Started"`
2. `"status": "running"`
3. `"start_date"` before restart and `"start_date"` after restart are NOT equal.

The agent is then performing the following commands on the host where the VM is running:

```
time="2024-10-22T08:21:01-05:00" level=info msg="1729603261391772000 /usr/local/bin/anka --machine-readable stop --force 0a7ec9c7-99d1-4c3e-af19-7a3b9a279ce6" logger=CMD
time="2024-10-22T08:21:02-05:00" level=info msg="1729603262827861000 /usr/local/bin/anka --machine-readable start 0a7ec9c7-99d1-4c3e-af19-7a3b9a279ce6" logger=CMD
```

##### Example

```shell
curl "http://anka.controller/api/v1/rpc/vm/restart" -d '{"instance_id":"c2af6080-3ae3-4cd3-64e5-2193f31b9aa5"}'
{"status":"OK","message":""}
```

#### List VM Instances

**Description:** List all VM instances  
**Path:** /api/v1/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id      | string | Return the VM with that ID. If the vm does not exists the server will return the status `FAIL`

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body*:  Array of Instances
- *message:* Error message in case of an error

##### Example

```shell
# List all VMs

curl  "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" | jq
{
   "status": "OK",
   "body": [
      {
  "status": "OK",
  "message": "",
  "body": [
    {
      "external_id": "TEST123",
      "instance_id": "e51259fe-6277-4776-4de0-016ea5971c7e",
      "name": null,
      "vm": {
        "instance_id": "e51259fe-6277-4776-4de0-016ea5971c7e",
        "instance_state": "Started",
        "anka_registry": "http://anka.registry:8089",
        "vmid": "d792c6f6-198c-470f-9526-9c998efe7ab4",
        "tag": "vanilla+port-forward-22+brew-git",
        "vminfo": {
          "uuid": "27f9cae9-08c4-48ad-8a06-0c10d66a45aa",
          "name": "mgmtManaged-14.2-arm64-1706114704759425000",
          "status": "running",
          "version": "",
          "creation_date": "2023-12-11T20:18:25Z",
          "access_date": "2024-01-24T16:51:54Z",
          "cpu_cores": 4,
          "ram": "4G",
          "node_id": "dc76c575-20dc-48be-a2a5-4f672fd758c6",
          "host_ip": "192.168.0.200",
          "ip": "192.168.69.15",
          "vnc_port": 5900,
          "port_forwarding": [
            {
              "guest_port": 22,
              "host_port": 10001,
              "protocol": "tcp",
              "name": "ssh"
            }
          ],
          "start_date": "2024-10-13T10:51:34Z",
          "stop_date": "0001-01-01T00:00:00Z",
          "timestamp": "2024-01-24T11:51:54.502527-05:00"
        },
        "node_id": "dc76c575-20dc-48be-a2a5-4f672fd758c6",
        "inflight_reqid": "ace1715e-ace7-4ae8-718e-825c02f579c5",
        "ts": "2024-01-24T11:51:56.935646-05:00",
        "cr_time": "2024-01-24T11:44:59.489699-05:00",
        "progress": 0,
        "external_id": "TEST123",
        "arch": "arm64",
        "vlan": ""
    },
    {
      "vm": {
        "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
        "instance_state": "Started",
        "cr_time": "2019-12-25T16:06:04.658586219Z",
        "vmid": "6b135004-0c89-43bb-b892-74796b8d266c",
        "vminfo": {
            "status": "running",
            "cpu_cores": 2,
            "ip": "",
            "name": "mgmtManaged-mojave-w-java-1577289969489444000",
            "vnc_port": 5903,
            "ram": "2G",
            "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
            "uuid": "4284cb1a-246c-4a2f-b773-ab18d6785957",
            "host_ip": "123.123.123.123",
            "vnc_connection_string": "vnc://:@123.123.123.123:5903",
            "start_date": "2024-10-13T10:51:34Z",
            "stop_date": "0001-01-01T00:00:00Z",
            "timestamp": "2024-10-13T10:51:34.658586219Z"
        },
        "tag": "teamcity",
        "anka_registry": "",
        "ts": "2019-12-25T16:06:20.717627395Z",
        "progress": 0
      },
      "instance_id": "6d5b7632-6ace-433a-44d9-e0c25f56706b"
    },
    {
      "vm": {
        "tag": "teamcity",
        "vminfo": {
            "ip": "",
            "tag": "teamcity",
            "status": "running",
            "cpu_cores": 2,
            "name": "mojave-w-java-1577289976968997000",
            "vnc_port": 5904,
            "ram": "2G",
            "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
            "host_ip": "123.123.123.123",
            "vnc_connection_string": "vnc://:admin@123.123.123.123:5904",
            "uuid": "bfd4106c-2a88-462f-a545-50f832b7a0f7",
            "start_date": "2024-10-13T10:51:34Z",
            "stop_date": "0001-01-01T00:00:00Z",
            "timestamp": "2024-10-13T10:51:34.658586219Z"
        },
        "progress": 1,
        "anka_registry": "",
        "ts": "2019-12-25T16:06:21.163835272Z",
        "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
        "cr_time": "2019-12-25T16:06:04.763922869Z",
        "vmid": "6b135004-0c89-43bb-b892-74796b8d266c",
        "instance_state": "Started"
      },
      "instance_id": "9c623cc1-e489-4d02-4559-0c76492bec9f"
    }
  ],
  "message": ""
}

# Show specific VM

curl "http://anka.controller/api/v1/vm?id=04b7ca7a-945c-4bdc-5123-68b2e4c8ad13" -H "Content-Type: application/json" 

{
   "message": "",
   "status": "OK",
   "body": {
      "progress": 0,
      "vminfo": {
         "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
         "cpu_cores": 2,
         "ip": "192.168.64.4",
         "status": "running",
         "uuid": "381e5c9f-d453-48dc-87a4-af6a2a98e46f",
         "port_forwarding": [
            {
               "host_port": 10001,
               "Name": "ssh",
               "protocol": "tcp",
               "guest_port": 22
            }
         ],
         "vnc_connection_string": "vnc://:@123.123.123.123:5902",
         "host_ip": "123.123.123.123",
         "ram": "2G",
         "name": "mgmtManaged-build_vm3-MacPro-02.local-1574781776830830000",
         "vnc_port": 5902
      },
      "vmid": "a29dcb72-f663-469d-9d83-8c4641b2e5dd",
      "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
      "anka_registry": "",
      "ts": "2019-12-26T08:57:48.392701636Z",
      "instance_state": "Started",
      "cr_time": "2019-11-26T15:22:52.221924626Z",
      "tag": "t1"
   }
}
```

---

### Node

**Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
node_id        | string | The node's id
node_name      | string | The node's name
cpu_count      | int    | Quantity of CPU cores
ram            | int    | Amount of RAM in GB
vm_count       | int    | Number of VMs currently running
vcpu_count     | int    | Number of running virtual CPUs
vram           | int    | Amount of virtual RAM used
cpu_util       | float  | CPU utilization of currently running Instances (0.0-1.0)
ram_util       | float  | RAM utilization of currently running Instances (0.0-1.0)
ip_address     | string | The IP address or host name of the node
state          | string | State of the node values can be "Offline", "Inactive (Invalid License)", "Active", "Updating", "Unhealthy"
capacity       | int    | Number of VMs the node can run
groups         | list   | List of groups that the nodes belongs to
anka_version   | object | An object representing the Anka information. running version, product name, license.
usb_devices    | list   | List of USB devices connected to the node
capacity_mode  | string | Node's capacity mode, options are `number` (number of running VMs) or `resource` (VCPUs and VRAM)
vcpu_override  | int    | VCPU scheduling limit, when using capacity mode `resource`  
ram_override   | int    | VRAM scheduling limit, when using capacity mode `resource`
disk_size      | int    | Node's disk size in bytes
free_disk_space | int   | Node's free disk space in bytes
anka_disk_usage | int   | Disk space used by Anka in bytes
templates      | array of objects | List of templates that the node has, each template has `name`, `tag` and `uuid`

#### List Nodes

**Description:** List all build nodes joined to the controller  
**Path:** /api/v1/node  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id      | string | Return the Node with that ID. If the node does not exists the server will return the status `FAIL`

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body*:  Array of Nodes
- *message:* Error message in case of an error

##### Examples

```shell
# List Nodes
❯ curl http://anka.controller:8090/api/v1/node | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   924  100   924    0     0   279k      0 --:--:-- --:--:-- --:--:--  300k
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "node_id": "32c6cf26-009c-4aec-97c9-a5832ce1452c",
      "node_name": "Nathans-MBP.attlocal.net",
      "cpu_count": 6,
      "ram": 36,
      "vm_count": 0,
      "vcpu_count": 0,
      "vram": 0,
      "cpu_util": 0.27960214,
      "ram_util": 0,
      "ip_address": "host.docker.internal",
      "state": "Active",
      "capacity": 2,
      "anka_version": {
        "product": "Anka Build Enterpriseplus",
        "version": "3.5.0",
        "build": "191",
        "license": "com.veertu.anka.entplus",
        "expires": "17-aug-2025"
      },
      "capacity_mode": "number",
      "vcpu_override": 0,
      "ram_override": 0,
      "disk_size": 994662584320,
      "free_disk_space": 306762862592,
      "anka_disk_usage": 132683026432,
      "host_arch": "arm64",
      "templates": [
        {
          "uuid": "0222a870-0b8c-44b3-989c-775f5cd0785f",
          "name": "14.5-arm64-jre17.48.15",
          "tag": "v1"
        },
        {
          "uuid": "d792c6f6-198c-470f-9526-9c998efe7ab4",
          "name": "14.5-arm64",
          "tag": "vanilla+port-forward-22+brew-git"
        }
      ],
      "last_updated": "2024-09-24T13:27:31.317822-08:00",
      "timestamp": 1727213251,
      "agent_version": "1.43.0-9f1c073a"
    }
  ]
}

# List Nodes (with groups assigned in the Controller; Enterprise and Enterprise+ license feature)
curl "http://anka.controller/api/v1/node" -H "Content-Type: application/json" | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "node_id": "3c101836-9540-4733-9482-604d0c5fbe30",
      "node_name": "Veertu.local",
      "cpu_count": 8,
      "ram": 32,
      "vm_count": 0,
      "vcpu_count": 0,
      "vram": 0,
      "cpu_util": 0.09658847,
      "ram_util": 0,
      "ip_address": "192.168.0.106",
      "state": "Active",
      "capacity": 2,
      "groups": [
        {
          "id": "bc288727-676c-4ab5-47b3-d1cd04227d11",
          "name": "iOS",
          "description": "Used for iOS builds and tests",
          "fallback_group_id": null
        }
      ],
      "anka_version": {
        "product": "Anka Build Enterpriseplus",
        "version": "2.2.3",
        "build": "118",
        "license": "com.veertu.anka.entplus"
        "expires": "17-aug-2025"
      },
      "usb_devices": null,
      "capacity_mode": "number",
      "vcpu_override": 0,
      "ram_override": 0,
      "disk_size": 1000240963584,
      "free_disk_space": 387449434112,
      "anka_disk_usage": 37276504000,
      "templates": [
        {
          "name": "11.2.3-openjdk-1.8.0_242-jenkins",
          "tag": "base",
          "uuid": "c0847bc9-5d2d-4dbc-ba6a-240f7ff08032"
        }
      ],
    }
  ]
}

# Show specific Node
curl "http://anka.controller/api/v1/node?id=f8707005-4630-4c9c-8403-c9c5964097f6" -H "Content-Type: application/json" | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "node_id": "32c6cf26-009c-4aec-97c9-a5832ce1452c",
      "node_name": "Nathans-MBP.attlocal.net",
      "cpu_count": 6,
      "ram": 36,
      "vm_count": 0,
      "vcpu_count": 0,
      "vram": 0,
      "cpu_util": 0.2630597,
      "ram_util": 0,
      "ip_address": "host.docker.internal",
      "state": "Active",
      "capacity": 2,
      "anka_version": {
        "product": "Anka Build Enterpriseplus",
        "version": "3.5.0",
        "build": "191",
        "license": "com.veertu.anka.entplus",
        "expires": "17-aug-2025"
      },
      "capacity_mode": "number",
      "vcpu_override": 0,
      "ram_override": 0,
      "disk_size": 994662584320,
      "free_disk_space": 306627973120,
      "anka_disk_usage": 132683026432,
      "host_arch": "arm64",
      "templates": [
        {
          "uuid": "0222a870-0b8c-44b3-989c-775f5cd0785f",
          "name": "14.5-arm64-jre17.48.15",
          "tag": "v1"
        },
        {
          "uuid": "d792c6f6-198c-470f-9526-9c998efe7ab4",
          "name": "14.5-arm64",
          "tag": "vanilla+port-forward-22+brew-git"
        }
      ],
      "last_updated": "2024-09-24T13:28:41.52849-08:00",
      "timestamp": 1727213321,
      "agent_version": "1.43.0-9f1c073a"
    }
  ]
}
```

#### Update Node

**Description:** Update Node configuration parameters.  
**Path:** /api/v1/node/config  
**Method:** POST  
**Required Data Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 node_id   | string | The specified Node's id.

 **Optional Data Parameters:**

 Parameter     | Type   | Description              | Default
 ---           | ---    |   ---                    | ---
 max_vm_count  | int    | Set Node capacity        | -
 name          | string | Set the Node's name      | -
 host          | string | Set the Node's host name | -
 capacity_mode | string | Set the Node's capacity mode, options are `number` (number of running VMs) or `resource` (VCPUs and VRAM) | Default behavior is `number`
 vcpu_override | int    | When using capacity mode `resource` set the VCPU scheduling limit | -
 ram_override  | int    | When using capacity mode `resource` set the VRAM scheduling limit | -
 disable_central_logging | bool | If true, disables central logging (logs will not be sent to the log server) | -
 drain_mode | bool | Puts the Node into [drain mode]({{< relref "whats-new/build-cloud-1.32.0/index.md#drain-mode" >}}) | -

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X POST "http://anka.controller/api/v1/node/config" -H "Content-Type: application/json" \
 -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6", "name": "MacPro1", "max_vm_count": 6}' | jq
{
   "status":"OK",
   "message":""
}

❯ curl -X POST http://anka.controller:8090/api/v1/node/config -d '{"node_id": "3c101836-9540-4733-9482-604d0c5fbe30", "drain_mode": true}'
{"status":"OK","message":""}
```

#### Delete Node

> Note
> To remove a Node from the cluster, execute `ankacluster disjoin` on the Node.

**Description:** Remove Nodes that do not exist anymore or have crashed  
**Path:** /api/v1/node  
**Method:** DELETE  
**Required Data Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 node_id   | string | The specified Node's id.

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X DELETE "http://anka.controller/api/v1/node" -H "Content-Type: application/json" \
 -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6"}' | jq
{
   "status":"OK",
   "message":""
}
```

#### Force Node Agent Update

**Description:** Forces the current agent package to download and install across all nodes attached to the controller  
**Path:** /api/v1/node/update  
**Method:** PUT  
**Required Data Parameters:** N/A

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

---

### Template

**Template Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
id             | string | The ID of the template
name           | string | Template's name
size           | int    | Total size of all Template's files on the registry in bytes. Including images, state files (suspend state), configuration files for all tags/versions  

**Tag/Version Object Model:**

*Each template has multiple Tags/Versions*

Property       | Type   | Description
---            |   ---  | ---
tag            | string | Tag name of the version
number         | int    | Serial number of the version (between versions)
description    | string | Tag's description
images         | list   | A list of image file names that this tag contains
state_files    | list   | A list of state file names (suspend state) that this tag contains
state_file     | string | Name of the state file (in case there multiple state files)
size           | int    | Total size of all tag's files on registry in bytes.
nvram          | string | Name of the nvram file
config_file    | string | Name of the tag's config file

#### List Templates  

**Description:** List all templates stored in the Registry.  
**Path:** /api/v1/registry/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | Return a specific Template. Passing this parameter will show more information about the Template's tags
 apiVer  | string | Use "v1" to use Registry V1 API (faster for large deployments, without size and arch data)

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body*:  Array of Templates or Template (if id supplied)
- *message:* Error message in case of an error

**Template List format**

- *name:* Template's name
- *id:* Template's id
- *size:* Template's size
- *arch:* Template's architecture
- *last_pull:* Last pull date in epoch/unix seconds
- *last_push:* Last push date in epoch/unix seconds

**Template format**

- *name:* Template's name
- *id:* Template's id
- *versions:* List of versions
- *size:* Template's size
- *arch:* Template's architecture
- *last_pull:* Last pull date in epoch/unix seconds
- *last_push:* Last push date in epoch/unix seconds

**Version format**

- *tag:* The version's tag
- *number:* Serial number of the version
- *description:* The version's description
- *images:* List of image file names
- *state_files:* List of state file names (suspend images)
- *config_file:* Name of the version's VM config file
- *nvram:* Name of the VM nvram file
- *arch:* Template's architecture
- *last_pull:* Last pull date in epoch/unix seconds
- *last_push:* Last push date in epoch/unix seconds

##### Example  

```shell
# List example
curl "http://anka.controller/api/v1/registry/vm" | jq
{
   "message": "",
   "body": [
      {
         "id": "0bf1a7e8-be95-43d9-a0c8-68c6aed0f2dd",
         "name": "jenkins",
         "size": 16427892736,
         "arch": "amd64",
         "last_pull": 1727209082,
         "last_push": 1721141574
      },
      {
         "id": "1820b42d-6581-46af-bf42-f64caa1e9633",
         "name": "catalina",
         "size": 20643704832,
         "arch": "amd64",
         "last_pull": 1727209082,
         "last_push": 1721141574
      },
      {
         "id": "2fa0f10e-e91e-4665-8d42-00a39b9707de",
         "name": "Catalina-Xcode-11",
         "size": 17834520576,
         "arch": "amd64",
         "last_pull": 1727209082,
         "last_push": 1721141574
      },
      {
         "name": "CachedBuidMojave",
         "id": "36fc63bb-6841-4528-9480-a9c44dc2740d",
         "size": 15040651264,
         "arch": "amd64",
         "last_pull": 1727209082,
         "last_push": 1721141574
      },
      {
         "name": "android-2",
         "id": "59adf1a8-c239-11e8-821d-c4b301c47c6b",
         "size": 19698943312,
         "arch": "amd64",
         "last_pull": 1727209082,
         "last_push": 1721141574
      }
   ],
   "status": "OK"
}


# Get Single Template
❯ curl -s http://anka.controller:8090/api/v1/registry/vm\?id\=d937553f-ab5f-405a-8d91-198f5001794e | jq
{
  "status": "OK",
  "message": "",
  "body": {
    "id": "d937553f-ab5f-405a-8d91-198f5001794e",
    "name": "14.5-arm64-teamcity",
    "versions": [
      {
        "number": 0,
        "tag": "v1",
        "config_file": "d937553f-ab5f-405a-8d91-198f5001794e.yaml",
        "nvram": "nvram",
        "images": [
          "ac34d3acee6549b2bf4822a82546d04d.ank",
          "7b1103ba6bdb433282160ca4b4b7362e.ank",
          "2cce19c0585e48df95400cb35691e1cb.ank",
          "770d9ec09d6b4207acfd0dd84a2920d4.ank"
        ],
        "state_files": [
          "a5058396218943858021cb37b31c718c.ank"
        ],
        "description": "",
        "state_file": "a5058396218943858021cb37b31c718c.ank",
        "size": 28655484928,
        "arch": "",
        "last_pull": 1727208044,
        "last_push": 1725265564
      }
    ],
    "size": 28655484928,
    "arch": "arm64",
    "last_pull": 1727208044,
    "last_push": 1725265564
  }
}
```

#### Delete Template

**Description:** Delete a specific VM template and all associated tags from the registry  
**Path:** /api/v1/registry/vm  
**Method:** DELETE  
**Required Query Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | The Template's id.

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X DELETE "http://anka.controller/api/v1/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" | jq
{
   "status":"OK",
   "message":""
}
```

#### Revert Template Tag

{{< hint warning >}}
Reverting is a potentially dangerous operation. It will revert all tags which came AFTER the one you're targeting AND the target tag itself. We recommend a multi-template-single-tag approach, providing the flexibility to delete a template when it's no longer needed and not lose other layers.
{{< /hint >}}

**Description:** Reverts a Template's Tags. Will default to reverting the latest Tag if no Tag or version is specified. If specified, the target tag and all Tags that came after it will be deleted.
**Path:** /api/v1/registry/revert  
**Method:** DELETE  
**Required Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | The Template id.

**Optional Query Parameters**  

 Parameter | Type   | Description     | Default
 ---       |   ---  |          ---    |   ---
 tag       | string | The Tag to revert. Newer versions will also be deleted | Latest
 version   | int    | The number of the version to revert to, 0 indexed | Latest

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
# Delete latest version
curl -X DELETE  "http://anka.controller/api/v1/registry/revert?id=00510971-5c37-4a60-a9c6-ea185397d9b4" | jq
{
   "body": null,
   "message": "",
   "status": "OK"
}

# Revert to the first version of the template
curl -X DELETE  "http://anka.controller/api/v1/registry/revert?id=a3cc47f0-3a73-11e9-b515-c4b301c47c6b&number=0" | jq
{
   "status": "OK",
   "body": null,
   "message": ""
}

# Revert to a specific Tag
curl -X DELETE  "http://anka.controller/api/v1/registry/revert?id=a3cc47f0-3a73-11e9-b515-c4b301c47c6b&tag=p120190904183122" | jq
{
   "status": "OK",
   "body": null,
   "message": ""
}
```

#### Delete Template from Node\[s\]

**Description:** Deletes a VM Template from all (or specific) nodes  
**Path:** /api/v1/registry/vm/remove  
**Method:** DELETE  
**Required Query Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | The Template's id.

**Optional Data Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 node_ids        | string array | A list of nodes to delete the template from

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error
- *body:*
   Number of requests sent (int): "requests_sent"
   Node IDs that the request was sent to (string array): "node_ids"

##### Example

{{< include file="_partials/anka-build-cloud/whatsnew/1.20.0/delete-template-from-nodes.md" >}}

#### Distribute Template

> Note
> Group_id is only available if you are running Enterprise tier of Anka Build.  

**Description:** Distribute a specific VM template to all or some build nodes.  
**Path:**  /api/v1/registry/vm/distribute  
**Method:** POST  
**Required Data Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 template_id | string | Id of the Template to distribute

**Optional Data Parameters:**

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---
 tag       | string | Specify a Tag to distribute | Latest Tag
 version   | int    | Specify a version number to distribute | Latest
 node_ids  | string array | Choose specific Nodes to distribute the Template to | All
 group_id  | string | Distribute the Template to the specified Node Group (enterprise only) | -
 group  | string | Set the Resource Permissions Group (required when Resource Management is enabled) (enterprise only) | -

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *body:* Request id of the distribution request
- *message:* Error message in case of an error

##### Example

```shell
curl -X POST "http://anka.controller/api/v1/registry/vm/distribute" \
-d '{"template_id": "eaef3af8-cb54-4c3e-baf9-839053472f15"}' | jq
{
   "body": {
      "request_id": "74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5"
   },
   "message": "",
   "status": "OK"
}
```

#### Get distribution status

**Description:** Get the status of a requested distribution  
**Path:** /api/v1/registry/vm/distribute  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | Return the Distribution request with that ID (`request_id`).

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body*:  List of request status or single request status
- *message:* Error message in case of an error

**Request Status format**
- *distribute_status* - Map of nodes
  - *status:* A boolean, true if the download is finished on the Node
  - *error:* Error message if the download failed on the Node
  - *progress:* A number representing the download progress
- *request* - An object that represents the request
  - *request_id:* The request's id
  - *template_id:* The Template being distributed
  - *time_requested:* The time of the request

##### Example

```shell
# List all distribution requests
curl  "http://anka.controller/api/v1/registry/vm/distribute" | jq
{
   "message": "",
   "body": [
      {
         "request": {
            "time_requested": "2019-12-26T17:48:50.380949005+02:00",
            "template_id": "eaef3af8-cb54-4c3e-baf9-839053472f15",
            "request_id": "74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5"
         },
         "distribute_status": {
            "64230242-321c-442a-bd96-d87edd0943a3": {
               "progress": 0,
               "status": false
            }
         }
      },
      {
        "request": {
          "time_requested": "2019-12-26T17:48:50.380949005+02:00",
          "template_id": "fdfa-fdfa-fdfa-fdfa-fdfafdfafdfa",
          "request_id": "42d42d42-d42d-42d4-2d42-d42d42d42d42"
        },
        "distribute_status": {
          "i-092bc39e9268d3b54": {
            "status": false,
            "error": "request canceled",
            "progress": 0.04
          }
        }
      }
    ],
    "status": "OK"
}

# Get a specific distribution request
curl  "http://anka.controller/api/v1/registry/vm/distribute?id=74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5" | jq
{
   "status": "OK",
   "body": {
      "distribute_status": {
         "64230242-321c-442a-bd96-d87edd0943a3": {
            "progress": 0,
            "status": false
         }
      },
      "request": {
         "request_id": "74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5",
         "template_id": "eaef3af8-cb54-4c3e-baf9-839053472f15",
         "time_requested": "2019-12-26T17:48:50.380949005+02:00"
      }
   },
   "message": ""
}
```


#### Cancel Distribution

**Description:** Cancel a distribution request  
**Path:** /api/v1/registry/vm/distribute  
**Method:** DELETE  
**Required Query Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | The Distribution request's id.

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error


#### Save Template Image

**Description:** Create a "Save Image" request. Save a running VM instance to the Registry as a new Tag or Template.

{{< hint warning >}}
- Upon successful push, the instance you have running will no longer exist/be running.
- Upon failure, the instance will still exist/be running for your to review.
- If using group permissions, you need to have `revert` permissions for all credentials involved in the request. This means the credential for both the API call and also the Node/Host that is running the VM.
{{< /hint >}}

**Path:** /api/v1/image  
**Method:** POST  
**Required Data Parameters:**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id                 | string | The ID of a running Controller Instance you want to save to the registry
 tag                | string  | The tag name to set
 new_template_name  | string  | Create a new VM Template with a specific name. *Required if target_vm_id not supplied*

 **Optional Data Parameters:**

 Parameter          | Type    | Description              | Default
 ---                | ---     |   ---                    | ---
 target_vm_id       | string  | The template id to save the tag to | -
 description        | string  | The description to use for the new tag | -
 suspend            | bool    | If true, suspends the vm before pushing | true
 script             | string  | Script to be executed before the instance is stopped/suspended, encoded as a base64 string. | -
 revert_before_push | bool    | If `target_vm_id` is not empty, revert the latest tag of the template or tag specified in `revert_tag` | false
 revert_tag         | string  | Revert this specific tag. In case the tag does not exist, revert operation does not take place. | -
 do_suspend_sanity_test | bool | If suspend is true, perform a suspend sanity test before pushing the VM to the registry | false
 cancel_on_script_failure | bool | Don't save the image if the script does not have `return_code: 0` in `GET /api/v1/image: script_result` (see below) | false

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body:* The created request id
- *message:* Error message in case of an error

##### Example

```shell
curl -X POST "http://anka.controller/api/v1/image" -H "Content-Type: application/json" \
-d '{"id": "cfc3cafd-d326-459d-7b3b-3c41b1a3efb7", "target_vm_id": "cb1473f2-1f0a-413c-a376-236bfd7d718f", "tag": "my-new-vm-1901", "revert_before_push": true, "revert_tag": "latest-vm-1801"}' | jq
{
   "status": "OK",
   "body": {
      "request_id": "cc55f7dd-5280-4120-461c-9b0ef9b40131"
   },
   "message": ""
}

❯ echo '#!/bin/bash
echo $(hostname)
echo
env
export TEST=true
bash -c "echo $TEST"' | base64 -i -
IyEvYmluL2Jhc2gKZWNobyAkKGhvc3RuYW1lKQplY2hvCmVudgpleHBvcnQgVEVTVD10cnVlCmJhc2ggLWMgImVjaG8gJFRFU1QiCg==

❯ curl -X POST "http://anka.controller/api/v1/image" -H "Content-Type: application/json" -d '{"id": "6f1776ad-3b6d-40b3-4bf9-21fe986bc26d", "target_vm_id": "e2729233-fbca-4e71-82de-511028191e33", "tag": "test123", "script": "IyEvYmluL2Jhc2gKZWNobyAkKGhvc3RuYW1lKQplY2hvCmVudgpleHBvcnQgVEVTVD10cnVlCmJhc2ggLWMgImVjaG8gJFRFU1QiCg==" }' | jq
{
   "status": "OK",
   "body": {
      "request_id": "ad84c5aa-2b1d-4ff5-72ef-9b519646b212"
   },
   "message": ""
}

```

#### List Save Template Image Requests

**Description:** Get a list of Save Image requests, or specify an id and get a single Save Image request.  
**Path:** /api/v1/image  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id      | string | Return only the Save Image request with that ID.

 **Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body:* Array of Save Image requests, or Single Save Image request
- *message:* Error message in case of an error

**Save Image request format**

- *id* - The request’s id
- *status* - The request's current status. Options are pending/done/error
- *script_result* - Details about what was executed in `script`. Example: `{ "stdout": "", "stderr": "", "errors": [], "return_code": 0, "did_timeout": false }`
  - `errors` - An array of errors that occurred while executing the script if `stdin` of `anka run` fails somehow
  - `stderr` - The stderr output of your script
  - `stdout` - The stdout output of your script
  - `return_code` - The return code of your script
- *request* - An object that represents the request
- *error* - Error message if status is error (controller errors, like with decoding responses). Examples: 
  ```
    "status": "error",
    "error": {
      "Message": "illegal base64 data at input byte 108",
      "Type": "base64.CorruptInputError"
    },
    "script_result": null
  ```

  With `cancel_on_script_failure` set to `true` and `script` set to `echo 'fadsfsd' | base64 -i -`
  ```
    "status": "error",
    "error": {
      "Message": "Script execution failed",
      "Code": 0
    },
    "script_result": {
      "return_code": 127,
      "errors": null,
      "did_timeout": false,
      "stdout": "",
      "stderr": "sh: line 1: fadsfsd: command not found\n"
    }
  ```

##### Example

```shell
# List all requests

❯ curl -s "http://anka.controller/api/v1/image" | jq '.body[1]'

{
  "id": "ad84c5aa-2b1d-4ff5-72ef-9b519646b212",
  "request": {
    "RequestId": "ad84c5aa-2b1d-4ff5-72ef-9b519646b212",
    "Priority": 0,
    "Timestamp": 1617720316,
    "ReserveExpiryTimestamp": 0,
    "RegistryAddr": "http://anka.registry:8089",
    "VMID": "bd07b49e-201c-44ad-8103-be39ec1b9a2c",
    "InstanceID": "6f1776ad-3b6d-40b3-4bf9-21fe986bc26d",
    "NodeID": "3c101836-9540-4733-9482-604d0c5fbe30",
    "TemplateID": "e2729233-fbca-4e71-82de-511028191e33",
    "NewTemplateName": "",
    "Tag": "test123",
    "Description": "",
    "Suspend": true,
    "Script": "IyEvYmluL2Jhc2gKZWNobyAkKGhvc3RuYW1lKQplY2hvCmVudgpleHBvcnQgVEVTVD10cnVlCmJhc2ggLWMgImVjaG8gJFRFU1QiCg==",
    "DoSuspendTest": false,
    "RevertBeforePush": false,
    "RevertTag": "",
    "CancelOnScriptFailure": false
  },
  "status": "done",
  "error": null,
  "script_result": {
    "stdout": "mgmtmanaged-11-2-3-openjdk-1-8-0-242-teamcity-1617720254314199000.local\n\nSHELL=/bin/bash\nTMPDIR=/var/folders/by/jq7dzyxj0151058zdlzj_st00000gn/T/\nUSER=anka\nSSH_AUTH_SOCK=/private/tmp/com.apple.launchd.hregPBnx2b/Listeners\n__CF_USER_TEXT_ENCODING=0x1F5:0x0:0x0\nPATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin\nPWD=/Users/anka\nXPC_FLAGS=0x0\nXPC_SERVICE_NAME=0\nSHLVL=2\nHOME=/Users/anka\nLOGNAME=anka\n_=/usr/bin/env\ntrue\n",
    "stderr": "",
    "errors": null,
    "did_timeout": false,
    "return_code": 0
  }
}

# Get specific request

curl "http://anka.controller/api/v1/image?id=cc55f7dd-5280-4120-461c-9b0ef9b40131" | jq
{
   "status": "OK",
   "message": "",
   "body": {
      "status": "pending",
      "request": {
         "Tag": "my-new-vm-1901",
         "Script": "",
         "Timestamp": 1577364097,
         "Priority": 0,
         "RevertBeforePush": true,
         "RequestId": "cc55f7dd-5280-4120-461c-9b0ef9b40131",
         "VMID": "578a3be9-ad37-49c3-99fc-59c41b79dc59",
         "RevertTag": "latest-vm-1801",
         "TemplateID": "cb1473f2-1f0a-413c-a376-236bfd7d718f",
         "NewTemplateName": "",
         "Suspend": true,
         "RegistryAddr": "http://192.168.1.108:8089",
         "DoSuspendTest": false,
         "NodeID": "64230242-321c-442a-bd96-d87edd0943a3",
         "InstanceID": "cfc3cafd-d326-459d-7b3b-3c41b1a3efb7",
         "Description": ""
      },
      "error": null,
      "id": "cc55f7dd-5280-4120-461c-9b0ef9b40131",
      "script_result": null
   }
}


```

---

### Group

#### Create Group

**Description:** Create a new node Group. Group is a "container" object used for grouping nodes.  
**Path:** /api/v1/group  
**Method:** POST  
**Required Data Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 name      | string | Name of the new Group

**Optional Data Parameters:**

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---
 description | string | Description for the Group | -
 fallback_group_id | string | Set a fallback Group | -

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *body:* The new Group object
- *message:* Error message in case of an error

##### Example

```shell
 curl -X POST "http://anka.controller/api/v1/group" -d '{"name": "GreateNodes", "description": "best of my macs"}' | jq
{
   "status": "OK",
   "message": "",
   "body": {
      "fallback_group_id": null,
      "name": "GreateNodes",
      "description": "best of my macs",
      "id": "89a66167-62b1-42bb-5a0b-ff667906b8f5"
   }
}
```

#### List Groups

**Description:** List all groups  
**Path:** /api/v1/group  
**Method:** GET

**Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body*: Array of Group
- *message:* Error message in case of an error

**Group format**

- *id* - The group's id
- *name* - The group's name
- *description* - The group's description
- *fallback_group_id* - Id of a the fallback group (group that requests go to if the current group is in full capacity)

##### Example

```shell
curl "http://anka.controller/api/v1/group" | jq
{
   "message": "",
   "body": [
      {
         "fallback_group_id": null,
         "description": "best of my macs",
         "id": "89a66167-62b1-42bb-5a0b-ff667906b8f5",
         "name": "GreateNodes"
      }
   ],
   "status": "OK"
}
```

#### Update Group

**Description:** Sets one or more parameters of an existing Group object.  
**Path:** /api/v1/group  
**Method:** PUT  
**Required Query Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | The id of the Group to update

**Optional Data Parameters:**

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---
 name              | string | Set the name of Group
 description       | string | Set description for the Group | -
 fallback_group_id | string | Set a fallback Group | -

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X PUT "http://anka.controller/api/v1/group?id=89a66167-62b1-42bb-5a0b-ff667906b8f5" \
-d '{"name": "Creme-de-la-nodes", "description": "A selected group of my favorite computers"}' | jq
{
   "message": "",
   "status": "OK"
}
```

#### Delete Group

**Description:** Delete a Group.  
**Path:** /api/v1/group  
**Method:** DELETE  
**Required Query Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 id        | string | The id of the Group to delete

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X DELETE "http://anka.controller/api/v1/group?id=89a66167-62b1-42bb-5a0b-ff667906b8f5" | jq
{
   "message": "",
   "status": "OK"
}
```

#### Add Nodes to Group

**Description:** Add one or more Nodes to one or more Groups.  
**Path:** /api/v1/node/group  
**Method:** POST  

**Required Data Parameters:**

 Parameter | Type         | Description       | Default
 ---       | ---          |   ---             | ---
 group_ids | string array | List of group ids to add the nodes to. | []
 node_ids  | string array | The Node ID to add the group IDs to. Currently only supports a single ID. | []

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X POST "http://anka.controller/api/v1/node/group" \
-d '{"group_ids": ["e41d4b47-4c10-4264-5519-2d52af568bdd"], "node_ids": ["64230242-321c-442a-bd96-d87edd0943a3"]}' | jq
{
   "status": "OK",
   "message": ""
}
```

#### Remove nodes from a group

**Description:** Remove one or more Nodes from one or more Groups.  
**Path:** /api/v1/node/group  
**Method:** DELETE  

**Required Data Parameters:**

 Parameter | Type         | Description       | Default
 ---       | ---          |   ---             | ---
 group_ids | string array | List of group ids to remove the nodes from. | []
 node_ids  | string array | The Node ID to remove the group IDs from. Currently only supports a single ID. | []

**Returns:**  

- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error

##### Example

```shell
curl -X DELETE "http://anka.controller/api/v1/node/group" \
-d '{"group_ids": ["e41d4b47-4c10-4264-5519-2d52af568bdd"], "node_ids": ["64230242-321c-442a-bd96-d87edd0943a3"]}' | jq
{
   "status": "OK",
   "message": ""
}
```

---

### USB

#### List Devices

**Description:** Get a list of all USB devices attached to the cloud  
**Path:** /api/v1/usb  
**Method:** GET  

**Returns:**

- *status:* Operation Result (OK|FAIL)  
- *Body*: Map of USB devices
- *message:* Error message in case of an error

**USB map entry format**
Each key is the device group name

- *available:* Number of available devices of this key
- *busy:* Number of busy devices of this key

##### Example

```shell
curl "http://anka.controller/api/v1/usb" | jq
{
   "message": "",
   "body": {
      "iphone7": {
         "available": 3,
         "busy": 1
      },
      "tablet": {
         "available": 0,
         "busy": 2
      }
   },
   "status": "OK"
}
```

---

### MAC Addresses Management

#### List Addresses

**Description:** Lists or counts mac address  
**Path:** /api/v1/macaddr  
**Method:** GET  

> This endpoint requires either `count/in_use` or `limit/last`

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| count | int | Return the number of mac addresses available | - |
| in_use | int | Return only the number of mac addresses in use | - |
| limit | string | Return a list of mac addresses and limit the results to the amount specified (ex: `in_use=1,limit=500`) | - |
| last | string | (pagination) Return the amount of items specified in `limit`, but start from a specific MAC address (ex: `limit=500&last=00:00:00:00:FF:FF`) |

**Returns:**

- *status:* Operation Result (OK|FAIL)
- *message:* Error message in case of an error

#### Force population of internal MAC list

**Description:** Triggers the internal MAC address population (useful if DELETE was submitted with API call)  
**Path:** /api/v1/macaddr  
**Method:** PUT  

**Returns:**

- *status:* Operation Result (OK|FAIL)
- *message:* Error message in case of an error

#### Force purge of internal MAC list

**Description:** Deletes all internal MAC addresses (run this before PUT to re-generate your initial range)  
**Path:** /api/v1/macaddr  
**Method:** DELETE  

**Returns:**

- *status:* Operation Result (OK|FAIL)
- *message:* Error message in case of an error

---

### VLAN

#### Get all VLANs IDs

**Description:** Gets the available VLAN ids in all hosts  
**Path:** /api/v1/vlan  
**Method:** GET  
**Query parameters:** None  
**Returns:**

- **status:** operation result
- **message:** Error message in case of error
- **body:** vlan ids as List of string

##### Example

```bash
curl http://anka.controller/api/v1/vlan
{
  "status": "OK",
  "message": "",
  "body": ["12", "55"]
}
```

### User Key Management

{{< hint warning >}}
Only available when [UAK/TAP]({{< relref "anka-build-cloud/Advanced Security Features/uak-tap-authentication.md" >}}) is enabled.
{{< /hint >}}


#### Get Key

- **Description:** Display information about api key\[s\]

- **Path:** /api/v1/apikey

- **Method:** GET

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | Return only information about a specific ID (optional) | - |

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error
- `body`: JSON returned from API

```bash
❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.controller:8090/api/v1/apikey | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "id": "developer1",
      "groups": [
        "testgroup"
      ],
      "expiration": "2021-09-21T14:35:08.823236-04:00",
      "created": "2021-09-20T14:35:08.823236-04:00"
    },
    {
      "id": "developer2",
      "groups": [
        "testgroup",
        "testgroup2"
      ],
      "expiration": "2021-09-21T14:39:00.526148-04:00",
      "created": "2021-09-20T14:39:00.526148-04:00"
    },
    {
      "id": "nathan",
      "groups": [
        "testgroup"
      ],
      "expiration": "2021-09-21T14:31:11.364844-04:00",
      "created": "2021-09-20T14:31:11.364845-04:00"
    }
  ]
}
```

#### Create Key

- **Description:** Create user key

- **Path:** /api/v1/apikey

- **Method:** POST

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | Set the id/name of the key (required) | - |
| ttl | int | Set the TTL (time to live) seconds of the key (required) | - |
| groups | string array | List of group names for the key (required) | - |
| publicKey | string | If supplied, it is expected to be in PKIX ASN.1 DER form (optional) | - |

{{< hint info >}}
If no `publicKey` is passed in, we will generate and store it on the server (controller: in etcd / registry: on disk) and then return the private key for you to store. It may be beneficial to create your key with `openssl` so that they are the same for both the registry and controller.
```bash
openssl genrsa -traditional -out $FILE_OUTPUT_DIR/$NAME-key.pem 4096 > /dev/null 2>&1
openssl rsa -in $FILE_OUTPUT_DIR/$NAME-key.pem -outform PEM -pubout -out $FILE_OUTPUT_DIR/$NAME-pub.pem > /dev/null 2>&1
# get PKIX ASN.1 DER form
cat $FILE_OUTPUT_DIR/$NAME-pub.pem | sed '1,1d' | sed '$d' | tr -d '\n'
```
{{< /hint >}}

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error
- `body`: JSON returned from API

```bash
❯ curl --insecure -X POST -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" https://anka.controller:8090/api/v1/apikey -d '{ "id": "nathan", "ttl": 86400, "groups": ["g1"] }'
{"status":"OK","message":"","body":"MIIEpAIBAAKCAQEA4vc30zDeKRCvyh4SjE1hMHH+gtts44fzAJCdUA9wxM/Hg6YvkqZTMDRC3ywGTpU3X4inObxWSSvqha/4xbsLzInlizatgTYJDJU7Tp8uozZOFYH2s/uQ7zVdFoYIJprGoILb/yKwbXNZMg9wSGhmR/rOnRjdcyqYLJP8/mO19JCtt+wcC0u5OSFEM7hrWORgFzeUvK/Y21obYgu+UvendPH20fGE1pm55gnIrjVxqdE8gZ7IbNRX/YqW257Pa4EfYizM+wrT0eJn+gHLrOAGG6YK+0RQRmuk/bwrXsbB1ZK0dxCzGP0TaAOb1MwK/g+RrqVhJlcOzpcTqyNEecEjdQIDAQABAoIBACWZgPUKrnMtIYIhUz9M/mHRMLGq+jIDbp1UV8tQk4T3Sv0jRdRMm5FrxvxDxdO04pSABfwJmF3M2bBGA7d2. . .
```

#### Update Key

- **Description:** Update an existing key

- **Path:** /api/v1/apikey

- **Method:** PUT

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | The id/name of the key (required) | - |
| groups | string array | List of group names for the key (required) | - |
| ttl | int | The TTL (time to live) seconds for the key (optional) | - |

> You cannot change the publicKey.

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error

```bash
❯ curl -X PUT -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.controller:8090/api/v1/apikey -d '{ "id": "developer3", "groups": ["test1","test3"] }'
{"status":"OK","message":""}
```

#### Delete Key

- **Description:** Delete an existing key

- **Path:** /api/v1/apikey

- **Method:** DELETE

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | The id/name of the key (required) | - |

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error

```bash
❯ curl -X DELETE -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.controller:8090/api/v1/apikey -d '{ "id": "developer3" }'
{"status":"OK","message":""}
```

---

### Resource Permissions

{{< hint warning >}}
Only available when [Resource Permissions for Permission Groups]({{< relref "anka-build-cloud/Advanced Security Features/authorization.md" >}}) is enabled.
{{< /hint >}}

#### Get Available Resources and Permissions

- **Description:** Returns a list of all available Resource types, Permissions, and their descriptions. The Controller and Registry differ and will return their individual Resource types.

- **Path:** /admin/api/v1/permission/resources/permissions

- **Method:** GET

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error
- `body`: An array of objects

```bash
❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64 -b 0)" http://anka.controller:8090/admin/v1/api/permission/resources/permissions | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "resource_type": "node",
      "permissions": {
        "change_config": {
          "label": "Change Node Configuration",
          "description": "Change Node Configuration"
        },
        "create_instance": {
          "label": "Create Instance",
          "description": "Create Instance"
        },
        "delete_template": {
          "label": "Delete Template from Node",
          "description": "Delete Template from Node"
        },
        "distribute": {
          "label": "Distribute Template to Node",
          "description": "Distribute Template to Node"
        },
        "force_upgrade": {
          "label": "Force Node Upgrade",
          "description": "Force Node Upgrade"
        },
        "list": {
          "label": "List Node",
          "description": "List Node"
        },
        "remove": {
          "label": "Remove Node",
          "description": "Remove Node"
        }
      }
    }
  ]
}

❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64 -b 0)" http://anka.registry:8089/admin/v1/api/permission/resources/permissions | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "resource_type": "template",
      "permissions": {
        "delete_revert": {
          "label": "Delete and/or Revert Template",
          "description": "Delete and/or Revert Template"
        },
        "get_config": {
          "label": "Get Template Configuration",
          "description": "Get Template Configuration"
        },
        "list": {
          "label": "List Template",
          "description": "List Template"
        },
        "pull": {
          "label": "Pull Template",
          "description": "Pull Template"
        },
        "push_tag": {
          "label": "Push Template Tag",
          "description": "Push Template Tag"
        },
        "save_image": {
          "label": "Save Image Target",
          "description": "Target Template for Save Image"
        },
        "set_arch": {
          "label": "Set Template Architecture",
          "description": "Set Template Architecture"
        }
      }
    }
  ]
}
```

#### Resource Model

**Object Model:**

Property       | Type   | Description
 ---           | ---    | ---
id    | string | Identifier for the resource. It should match the Node ID or Template ID and will display the proper name of the resource in the Controller UI if set properly.
id_type | string | Optional, but defaults to "id" (subject to change).
type | string | The type of resource this is. For example `node` or `template`.
permissions | array of strings | The permissions to enable for the specific resource and targeted group. [Get Available Resources and Permissions](#get-available-resources-and-permissions) to see what's possible.

#### Get Group Resources and Permissions

- **Description:** Returns a list of all currently set Resource Permissions for a specific group (`groupName`).

- **Path:** /admin/api/v1/permission/group/{groupName}/resources

- **Method:** GET

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error
- `body`: An array of objects

```bash
❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64 -b 0)" http://anka.controller:8090/admin/v1/api/permission/group/devops/resources | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "id": "3c101836-9540-4733-9482-604d0c5fbe30",
      "id_type": "id",
      "type": "node",
      "permissions": [
        "change_config",
        "create_instance",
        "delete_template",
        "distribute",
        "force_upgrade",
        "list",
        "remove"
      ]
    }
  ]
}

❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64 -b 0)" http://anka.registry:8089/admin/v1/api/permission/group/devops/resources | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "id": "e882dbfe-e6c2-4297-9932-c5efe9bb2322",
      "id_type": "id",
      "type": "template",
      "permissions": [
        "delete_revert",
        "get_config",
        "list",
        "pull",
        "push_tag",
        "save_image",
        "set_arch"
      ]
    },
    {
      "id": "5d1b40b9-7e68-4807-a290-c59c66e926b4",
      "id_type": "id",
      "type": "template",
      "permissions": [
        "delete_revert",
        "get_config",
        "list",
        "pull",
        "push_tag",
        "save_image",
        "set_arch"
      ]
    },
    {
      "id": "c0847bc9-5d2d-4dbc-ba6a-240f7ff08032",
      "id_type": "id",
      "type": "template",
      "permissions": [
        "delete_revert",
        "get_config",
        "list",
        "pull",
        "push_tag",
        "save_image",
        "set_arch"
      ]
    },
    {
      "id": "c12ccfa5-8757-411e-9505-128190e9854e",
      "id_type": "id",
      "type": "template",
      "permissions": [
        "delete_revert",
        "get_config",
        "list",
        "pull",
        "push_tag",
        "save_image",
        "set_arch"
      ]
    }
  ]
}
```

#### Create or Update Group Resources

- **Description:** Sets the Resource Permissions for a specific group (`groupName`). Input data structure is an array of Resource objects.

- **Path:** /admin/api/v1/permission/group/{groupName}/resources

- **Method:** POST

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Success or Error message

```bash
❯ curl -X POST -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64 -b 0)" http://anka.registry:8090/admin/v1/api/permission/group/devops/resources -d '[{
      "id": "3c101836-9540-4733-9482-604d0c5fbe30",
      "id_type": "id",
      "type": "node",
      "permissions": [
        "change_config",
        "create_instance",
        "delete_template",
        "distribute",
        "force_upgrade",
        "list"
      ]
    }]'
{"status":"OK","message":"Saved"}
```
