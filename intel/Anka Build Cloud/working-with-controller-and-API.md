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

#### Explanation of Node States
- **Offline:** Node has not checked in recently
- **Inactive:** (Invalid License): License has likely expired; log in to the node and run `anka license show`
- **Active:** Node is healthy
- **Updating:** Node is downloading and installing the proper agent pkg (if the controller has been upgraded)
- **Unhealthy:** VMs running on Node are in an errored or failed
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

## Enterprise License Features

### Node Groups

{{< include file="_partials/intel/Anka Build Cloud/_node_groups.md" >}}

### Priority Scheduling

{{< include file="_partials/intel/Anka Build Cloud/_priority-scheduling.md" >}}

### USB Device Control (Controller API)

{{< include file="_partials/intel/Anka Build Cloud/_usb-device-control-api.md" >}}

### Event logging and automated pushing

{{< include file="_partials/intel/Anka Build Cloud/_event-logging-endpoint-pushing.md" >}}

---

## REST API

Use the REST APIs to integrate Anka Build cloud with your CI system (If there is no plugin/integration available).

### Status

```bash
❯ curl -s http://anka.controller:8090/api/v1/status         
{"status":"OK","message":"","body":{"status":"Running","version":"1.13.0-24e848a5","registry_address":"http://anka.registry:8089","registry_status":"Running","license":"enterprise plus"}}
```
## VM Instance

**Object Model:**

Property       | Type   | Description 
 ---           | ---    | ---
instance_id    | string | identifier for the instance
instance_state | string | the instance's state options are "Scheduling", "Pulling", "Started", "Stopping", "Stopped", "Terminating", "Terminated", "Error", "Pushing" (Scheduling -> Pulling, Started, Stopped, Pushing, or Error -> Terminating -> Terminated)
message        | string | Error message in case of an error
anka_registry  | string | the URL of the registry where the template is saved
vmid           | string | the Id of the template that the VM is created from
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

> All fields but the following are omitted if empty: `vmid`, `group_id`, and `usb_device`

### Start VM instances 

> Note
> Group_id, priority and USB_device is only available if you are running Enterprise and higher tier of Anka Build.

**Description:** Start VM instances  
**Path:**   /api/v1/vm  
**Method:** POST  
**Required Body Parameters:**   

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 vmid      | string | The template (vm image) to use for the instance. 

**Optional Body Parameters:**   

Parameter | Type   | Description       | Default
---       | ---    |   ---             | --- 
tag       | string | Specify a specific tag to use | Latest tag.
version   | int    | Specify a version number instead of a tag. | -
name      | string | A name for the instance. | -
external_id | string | An arbitrary string to be saved with the instance | -
count     | int    | Number of instances to start. | 1
node_id   | string | Start the instance on this specific node | -
startup_script | string | Script to be executed after the instance is started, encoded as a base64 string. | -
startup_script_condition | int | Options are 0 or 1. If 0 is passed the script will run after the VM's network is up, if 1 is passed the script will run as soon as the VM boots. | wait for network 
name_template | string | Name template for the vm name (on the Node), available vars are $template_name $template_id and $ts (timestamp) | -
group_id  | string | Run the VM on a node from this group. | -
priority  | int    | Priority of this instance in range 1-10000 (lower is more urgent). | 1000
usb_device | string | Name of the USB device to attach to the VM | -
vcpu      |  int    | Override the number of CPU cores for the VM Template **(only works when the template VM is stopped)**.
vram      |  int    | Override the VM's RAM size in MB (1GB = 1024MB) **(only works when the template VM is stopped)**.
metadata  | object  | Sets the instance metadata, a key-value object. Keys are strings. Values are strings, ints or booleans | -
mac_address      |  string    | Specify MAC address for the VM (Capital letters and ':' as separators) **(only works when the template VM is stopped and when --manage-mac-addresses flag is set in the controller config)**.

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *body:* Array of instance UUIDs  
- *message:* Error message in case of an error 

#### Example
```shell
 curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
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

### Update Instance

**Description:** Update VM Instance
**Path:**  /api/v1/vm
**Method:** PUT
**Required Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id      | string | Return the VM with that ID. If the vm does not exists the server will return the status `FAIL` 

**Optional Body Parameters:**

 Parameter     | Type   | Description              | Default
 ---           | ---    |   ---                    | ---
 name          | string | A name for the instance  | -
 external_id   | string | An arbitrary string to be saved with the instance   | -
 metadata      | object | Updates the instance metadata, a key-value object. Keys are strings. Values are strings, ints or booleans | -

 **Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error 

#### Example
```shell
 curl -X PUT "http://anka.controller/api/v1/vm?id=c0f36a87-41d9-44a8-66e1-6c5afae15b80" -H "Content-Type: application/json" \
  -d '{"name": "My VM name"}'

{
  "status": "OK",
  "message": ""
}
```


### Terminate VM instance

**Description:** Terminate a running VM instance  
**Path:** /api/v1/vm  
**Method:** DELETE  
**Required Body Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id      | string | The id of the instance to terminate.

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 


#### Example
```shell
curl -X DELETE "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
-d '{"id": "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce"}' | jq
{
   "status": "OK",
   "message": ""
}
```

### List VM Instances

**Description:** List all VM instances  
**Path:** /api/v1/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id      | string | Return the VM with that ID. If the vm does not exists the server will return the status `FAIL` 


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  Array of Instances 
 - *message:* Error message in case of an error 

#### Example
```shell
# List all VMs

curl  "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" | jq
{
   "status": "OK",
   "body": [
      {
         "instance_id": "04b7ca7a-945c-4bdc-5123-68b2e4c8ad13",
         "name": "My VM",
         "external_id": "ly79930",
         "vm": {
            "anka_registry": "",
            "ts": "2019-12-25T16:06:20.681609561Z",
            "progress": 0,
            "vminfo": {
               "ip": "192.168.64.4",
               "status": "running",
               "cpu_cores": 2,
               "name": "mgmtManaged-build_vm3-MacPro-02.local-1574781776830830000",
               "port_forwarding": [
                  {
                     "Name": "ssh",
                     "protocol": "tcp",
                     "host_port": 10001,
                     "guest_port": 22
                  }
               ],
               "ram": "2G",
               "vnc_port": 5902,
               "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
               "vnc_connection_string": "vnc://:@123.123.123.123:5902",
               "host_ip": "123.123.123.123",
               "uuid": "381e5c9f-d453-48dc-87a4-af6a2a98e46f"
            },
            "tag": "t1",
            "instance_state": "Started",
            "cr_time": "2019-11-26T15:22:52.221924626Z",
            "vmid": "a29dcb72-f663-469d-9d83-8c4641b2e5dd",
            "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6"
         }
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
               "vnc_connection_string": "vnc://:@123.123.123.123:5903"
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
               "uuid": "bfd4106c-2a88-462f-a545-50f832b7a0f7"
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


## Node 

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
cpu_util       | float  | CPU utilization (0-1)
ram_util       | float  | RAM utilization (0-1)
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


### List Nodes

**Description:** List all build nodes joined to the controller  
**Path:** /api/v1/node  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id      | string | Return the Node with that ID. If the node does not exists the server will return the status `FAIL` 

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  Array of Nodes 
 - *message:* Error message in case of an error 

#### Examples
```shell
# List Nodes
curl "http://anka.controller/api/v1/node" -H "Content-Type: application/json" | jq
{
   "body": [
      {
        "ip_address": "123.123.123.123",
        "cpu_count": 12,
        "capacity": 6,
        "node_name": "MacPro-02.local",
        "vm_count": 1,
        "usb_devices": null,
        "vcpu_count": 2,
        "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
        "cpu_util": 0.074900396,
        "vram": 2048,
        "ram_util": 0.03125,
        "state": "Active",
        "vcpu_override": 0,
        "anka_version": {
          "build": "112",
          "product": "Anka Build Enterpriseplus",
          "version": "2.1.2",
          "license": "com.veertu.anka.entplus"
        },
        "free_disk_space": 155624185856,
        "anka_disk_usage": 38274388000,
        "disk_size": 250135076864,
        "ram": 64,
        "ram_override": 0,
        "capacity_mode": "number",
        "templates": [
          {
            "name": "11.2.3-openjdk-1.8.0_242-jenkins",
            "tag": "base",
            "uuid": "c0847bc9-5d2d-4dbc-ba6a-240f7ff08032"
          }
        ],
      }
   ],
   "status": "OK",
   "message": ""
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
  "message": "",
  "status": "OK",
  "body": [
    {
      "cpu_count": 12,
      "vcpu_override": 0,
      "capacity": 6,
      "usb_devices": null,
      "ram": 64,
      "ram_util": 0.03125,
      "state": "Active",
      "ip_address": "123.123.123.123",
      "vm_count": 1,
      "vram": 2048,
      "anka_version": {
        "license": "",
        "build": "",
        "product": "",
        "version": ""
      },
      "capacity_mode": "",
      "free_disk_space": 155624185856,
      "anka_disk_usage": 38274388000,
      "disk_size": 250135076864,
      "cpu_util": 0.08308069,
      "node_name": "MacPro-02.local",
      "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
      "ram_override": 0,
      "vcpu_count": 2,
      "templates": []
    }
  ]
}
```

### Update Node

**Description:** Update Node configuration parameters.  
**Path:** /api/v1/node/config  
**Method:** POST  
**Required Body Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 node_id   | string | The specified Node's id.

 **Optional Body Parameters:**   

 Parameter     | Type   | Description              | Default
 ---           | ---    |   ---                    | ---
 max_vm_count  | int    | Set Node capacity        | -
 name          | string | Set the Node's name      | -
 host          | string | Set the Node's host name | -
 capacity_mode | string | Set the Node's capacity mode, options are `number` (number of running VMs) or `resource` (VCPUs and VRAM) | Default behavior is `number` 
 vcpu_override | int    | When using capacity mode `resource` set the VCPU scheduling limit | -
 ram_override  | int    | When using capacity mode `resource` set the VRAM scheduling limit | -
 disable_central_logging | bool | If true, disables central logging (logs will not be sent to the log server) | -


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  Array of Nodes 
 - *message:* Error message in case of an error 

#### Example
```shell
curl -X POST "http://anka.controller/api/v1/node/config" -H "Content-Type: application/json" \
 -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6", "name": "MacPro1", "max_vm_count": 6}' | jq
{
   "status":"OK",
   "message":""
}
```

### Delete Node

> Note
> To remove a Node from the cluster, execute `ankacluster disjoin` on the Node. 

**Description:** Remove Nodes that do not exist anymore or have crashed  
**Path:** /api/v1/node  
**Method:** DELETE  
**Required Body Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 node_id   | string | The specified Node's id.

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 

#### Example
```shell
curl -X DELETE "http://anka.controller/api/v1/node" -H "Content-Type: application/json" \
 -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6"}' | jq
{
   "status":"OK",
   "message":""
}
```

### Force Node Agent Update

**Description:** Forces the current agent package to download and install across all nodes attached to the controller  
**Path:** /api/v1/node/update  
**Method:** PUT  
**Required Body Parameters:** N/A

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 


## Template

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


 
### List Templates  

**Description:** List all templates stored in the Registry.  
**Path:** /api/v1/registry/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return a specific Template. Passing this parameter will show more information about the Template's tags 


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  Array of Templates or Template (if id supplied)
 - *message:* Error message in case of an error 

**Template List format**
 - *name:* Template's name
 - *id:* Template's id

**Template format**
 - *name:* Template's name
 - *id:* Template's id
 - *versions:* Array of Version objects. 

**Version format**
 - *tag:* The version's tag
 - *number:* Serial number of the version
 - *description:* The version's description
 - *images:* List of image file names
 - *state_files:* List of state file names (suspend images)
 - *config_file:* Name of the version's VM config file
 - *nvram:* Name of the VM nvram file

#### Example  
```shell
# List example
curl "http://anka.controller/api/v1/registry/vm" | jq
{
   "message": "",
   "body": [
      {
         "id": "0bf1a7e8-be95-43d9-a0c8-68c6aed0f2dd",
         "name": "jenkins-slave",
         "size": 16427892736
      },
      {
         "id": "1820b42d-6581-46af-bf42-f64caa1e9633",
         "name": "catalina",
         "size": 20643704832
      },
      {
         "id": "2fa0f10e-e91e-4665-8d42-00a39b9707de",
         "name": "Catalina-Xcode-11",
         "size": 17834520576
      },
      {
         "name": "CachedBuidMojave",
         "id": "36fc63bb-6841-4528-9480-a9c44dc2740d",
         "size": 15040651264
      },
      {
         "name": "android-2",
         "id": "59adf1a8-c239-11e8-821d-c4b301c47c6b",
         "size": 19698943312
      }
   ],
   "status": "OK"
}


# Get Single Template
curl "http://anka.controller/api/v1/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" | jq
{
   "message": "",
   "body": {
      "name": "android-2",
      "id": "00510971-5c37-4a60-a9c6-ea185397d9b4",
      "versions": [
         {
            "config_file": "00510971-5c37-4a60-a9c6-ea185397d9b4.yaml",
            "state_files": [
               "c19ba955c706475e9aeade79f174a925.ank"
            ],
            "number": 0,
            "size": 16634568704,
            "description": "",
            "nvram": "nvram",
            "images": [
               "83e3eb9a2b694ddbb90f535ffae4cbb8.ank",
               "fae1423d7c99419c92109c326162c2dd.ank",
               "51f90db935494831a831dae51c9743e0.ank",
               "2be4266d24704db2bacbbd258d0d6288.ank",
               "2d07e328bfc449b58f74b3e08f8d049d.ank",
               "06ad0ac5-af7a-11e8-884c-c4b301c47c6b.ank",
               "237a78ab78254cde9f04f2cecaec21b7.ank",
               "7ae0e540-cae2-11e8-b0c8-c4b301c47c6b.ank",
               "916cd0f1087345659b70275bb8cc3101.ank",
               "239bb374545141ceb481d495ec01683e.ank",
               "4ef1b52304264ac2bacaa34903b7af9c.ank",
               "d2da3f9b4faa49b1804af2adccf5bccd.ank",
               "dd94fbf0-c6ed-11e8-920b-c4b301c47c6b.ank",
               "daba8902e1d24636bc424ac44252a090.ank",
               "d97298f5-a06c-11e8-964c-c4b301c47c6b.ank",
               "85ab1ffd-af80-11e8-bd5e-c4b301c47c6b.ank"
            ],
            "tag": "t1"
         }
      ]
   },
   "status": "OK"
}
```

### Delete Template

**Description:** Delete a specific VM template and all associated tags from the registry  
**Path:** /api/v1/registry/vm  
**Method:** DELETE  
**Required Query Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The Template's id.

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 

#### Example
```shell
curl -X DELETE "http://anka.controller/api/v1/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" | jq
{
   "status":"OK",
   "message":""
}
```

### Revert Template Tag

**Description:** Revert a Template to a certain Tag or version number. Delete the latest version if none is specified.  
**Path:** /api/v1/registry/revert  
**Method:** DELETE  
**Required Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The Template id. 

**Optional Query Parameters**  

 Parameter | Type   | Description     | Default
 ---       |   ---  |          ---    |   ---
 tag       | string | The Tag to revert to. Newer versions will also be deleted | Latest 
 version   | int    | The number of the version to revert to, 0 indexed | Latest

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 

#### Example
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

### Delete Template from Node\[s\]

**Description:** Deletes a VM Template from all (or specific) nodes  
**Path:** /api/v1/registry/vm/remove  
**Method:** DELETE  
**Required Query Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The Template's id.

**Optional Body Parameters:**

 Parameter | Type   | Description
 ---       |   ---  |          ---
 node_ids        | string array | A list of nodes to delete the template from

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 
 - *body:* 
   Number of requests sent (int): "requests_sent"
   Node IDs that the request was sent to (string array): "node_ids"

### Distribute Template

> Note
> Group_id is only available if you are running Enterprise tier of Anka Build.  

**Description:** Distribute a specific VM template to all or some build nodes.  
**Path:**  /api/v1/registry/vm/distribute  
**Method:** POST  
**Required Body Parameters:**   

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 template_id | string | Id of the Template to distribute

**Optional Body Parameters:**   

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---
 tag       | string | Specify a Tag to distribute | Latest Tag
 version   | int    | Specify a version number to distribute | Latest 
 node_ids  | string array | Choose specific Nodes to distribute the Template to | All
 group_id  | string | Distribute the Template to the specified group (enterprise) | -

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *body:* Request id of the distribution request
- *message:* Error message in case of an error 

#### Example
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

### Get distribution status 

**Description:** Get the status of a requested distribution  
**Path:** /api/v1/registry/vm/distribute  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return the Distribution request with that ID (`request_id`). 


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  List of request status or single request status
 - *message:* Error message in case of an error 

**Request Status format**
+ *distribute_status* - Map of nodes
  - *status:* A boolean, true if the download is finished on the Node
  - *progress:* A number representing the download progress
+ *request* - An object that represents the request
  - *request_id:* The request's id
  - *template_id:* The Template being distributed
  - *time_requested:* The time of the request

#### Example
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

### Save Template Image

**Description:** Create a "Save Image" request. Save a running VM instance to the Registry as a new Tag or Template.

> **The instance you have running will no longer exist/be running after using this Save Template Image.**

**Path:** /api/v1/image  
**Method:** POST  
**Required Body Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The ID of a running Controller Instance you want to save to the registry
 new_template_name  | string  | Create a new VM Template with a specific name. _Required if target_vm_id not supplied_ | -

 
 **Optional Body Parameters:**   

 Parameter          | Type    | Description              | Default
 ---                | ---     |   ---                    | ---
 target_vm_id       | string  | The template id to save the tag to | -
 tag                | string  | The tag name to set | -
 description        | string  | The description to use for the new tag | -
 suspend            | bool    | If true, suspends the vm before pushing | true 
 script             | string  | Script to be executed before the instance is stopped/suspended, encoded as a base64 string. | -
 revert_before_push | bool    | If `target_vm_id` is not empty, revert the latest tag of the template or tag specified in `revert_tag` | false
 revert_tag         | string  | Revert this specific tag. In case the tag does not exist, revert operation does not take place. | -
 do_suspend_sanity_test | bool | If suspend is true, perform a suspend sanity test before pushing the VM to the registry | false
 cancel_on_script_failure | bool | Don't save the image if the script does not have `return_code: 0` in `GET /api/v1/image: script_result` (see below) | false

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body:* The created request id 
 - *message:* Error message in case of an error 

#### Example
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

❯ curl -X POST "http://anka.controller:8090/api/v1/image" -H "Content-Type: application/json" -d '{"id": "6f1776ad-3b6d-40b3-4bf9-21fe986bc26d", "target_vm_id": "e2729233-fbca-4e71-82de-511028191e33", "tag": "test123", "script": "IyEvYmluL2Jhc2gKZWNobyAkKGhvc3RuYW1lKQplY2hvCmVudgpleHBvcnQgVEVTVD10cnVlCmJhc2ggLWMgImVjaG8gJFRFU1QiCg==" }' | jq
{
   "status": "OK",
   "body": {
      "request_id": "ad84c5aa-2b1d-4ff5-72ef-9b519646b212"
   },
   "message": ""
}

```

### List Save Template Image Requests

**Description:** Get a list of Save Image requests, or specify an id and get a single Save Image request.  
**Path:** /api/v1/image  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id      | string | Return only the Save Image request with that ID. 

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body:* Array of Save Image requests, or Single Save Image request
 - *message:* Error message in case of an error 

**Save Image request format**
- *id* - The request’s id
- *status* - The request's current status. Options are pending/done/error
- *script_result* - Details about what was executed in `script`. Example: `{ "stdout": "", "stderr": "", "errors": [], "return_code": 0 }`
- *request* - An object that represents the request
- *error* - Error message if status is error

#### Example
```shell
# List all requests

❯ curl -s "http://anka.controller:8090/api/v1/image" | jq '.body[1]'

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

## Group
### Create Group

**Description:** Create a new node Group. Group is a "container" object used for grouping nodes.  
**Path:** /api/v1/group  
**Method:** POST  
**Required Body Parameters:**   

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 name      | string | Name of the new Group

**Optional Body Parameters:**   

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---
 description | string | Description for the Group | -
 fallback_group_id | string | Set a fallback Group | -

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *body:* The new Group object
- *message:* Error message in case of an error 

#### Example
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

### List Groups

**Description:** List all groups  
**Path:** /api/v1/group  
**Method:** GET   

**Returns:** 
- *Status:* Operation Result (OK|FAIL)  
- *Body*: Array of Group 
- *message:* Error message in case of an error 

**Group format**
- *id* - The group's id
- *name* - The group's name
- *description* - The group's description
- *fallback_group_id* - Id of a the fallback group (group that requests go to if the current group is in full capacity)

#### Example
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

### Update Group

**Description:** Sets one or more parameters of an existing Group object.  
**Path:** /api/v1/group  
**Method:** PUT  
**Required Query Parameters:**

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The id of the Group to update

**Optional Body Parameters:**   

 Parameter | Type   | Description       | Default
 ---       | ---    |   ---             | ---
 name              | string | Set the name of Group
 description       | string | Set description for the Group | -
 fallback_group_id | string | Set a fallback Group | -

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error 

#### Example
```shell
curl -X PUT "http://anka.controller/api/v1/group?id=89a66167-62b1-42bb-5a0b-ff667906b8f5" \
-d '{"name": "Creme-de-la-nodes", "description": "A selected group of my favorite computers"}' | jq
{
   "message": "",
   "status": "OK"
}
```

### Delete Group

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

#### Example
```shell
curl -X DELETE "http://anka.controller/api/v1/group?id=89a66167-62b1-42bb-5a0b-ff667906b8f5" | jq
{
   "message": "",
   "status": "OK"
}
```

### Add Nodes to Group

**Description:** Add one or more Nodes to one or more Groups.  
**Path:** /api/v1/node/group  
**Method:** POST  

**Required Body Parameters:**   

 Parameter | Type         | Description       | Default
 ---       | ---          |   ---             | ---
 group_ids | string array | List of group ids to add the nodes to. | []
 node_ids  | string array | List of nodes to add to the specified groups. | []

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error 

#### Example
```shell
curl -X POST "http://anka.controller/api/v1/node/group" \
-d '{"group_ids": ["e41d4b47-4c10-4264-5519-2d52af568bdd"], "node_ids": ["64230242-321c-442a-bd96-d87edd0943a3"]}' | jq
{
   "status": "OK",
   "message": ""
}
```

### Remove nodes from a group


**Description:** Remove one or more Nodes from one or more Groups.  
**Path:** /api/v1/node/group  
**Method:** DELETE  

**Required Body Parameters:**   

 Parameter | Type         | Description       | Default
 ---       | ---          |   ---             | ---
 group_ids | string array | List of group ids to remove the nodes from. | []
 node_ids  | string array | List of nodes to remove from the specified group ids. | []

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error 

#### Example
```shell
curl -X DELETE "http://anka.controller/api/v1/node/group" \
-d '{"group_ids": ["e41d4b47-4c10-4264-5519-2d52af568bdd"], "node_ids": ["64230242-321c-442a-bd96-d87edd0943a3"]}' | jq
{
   "status": "OK",
   "message": ""
}
```
## USB
### List Devices

**Description:** Get a list of all USB devices attached to the cloud  
**Path:** /api/v1/usb  
**Method:** GET  

**Returns:** 
- *Status:* Operation Result (OK|FAIL)  
- *Body*: Map of USB devices 
- *message:* Error message in case of an error 

**USB map entry format**
Each key is the device group name
- *available:* Number of available devices of this key
- *busy:* Number of busy devices of this key

#### Example
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

## MAC Addresses Management

### List Addresses

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

### Force population of internal MAC list

**Description:** Triggers the internal MAC address population (useful if DELETE was submitted with API call)  
**Path:** /api/v1/macaddr  
**Method:** PUT  

**Returns:**
- *status:* Operation Result (OK|FAIL)
- *message:* Error message in case of an error

### Force purge of internal MAC list

**Description:** Deletes all internal MAC addresses (run this before PUT to re-generate your initial range)  
**Path:** /api/v1/macaddr  
**Method:** DELETE  

**Returns:**
- *status:* Operation Result (OK|FAIL)
- *message:* Error message in case of an error

