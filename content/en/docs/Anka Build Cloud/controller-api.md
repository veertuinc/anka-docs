
---
date: 2019-12-12T00:00:00-00:00
title: "Controller API Reference"
linkTitle: "Controller API Reference"
weight: 7
description: >
  Using the Controller REST API.
---

> Note
> Run these against IP:Port of the Controller.

Use the REST APIs to integrate Anka Build cloud with your CI system(If there is no plugin/integration available).

## VM Instance

**Object Model:**

Property       | Type   | Description 
 ---           | ---    | ---
instance_id    | string | identifier for the instance
instance_state | string | the instance's state options are "Scheduling", "Pulling", "Started", "Stopping", "Stopped", "Terminating", "Terminated", "Error", "Pushing"
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
 vcpu      |  int    | Override the number of the VM's CPU cores **only works when the template VM is stopped**.
 vram      |  int    | Override the VM's RAM size in MB **only works when the template VM is stopped**.
 metadata  | object  | Sets the instance metadata, a key-value object. Keys are strings. Values are strings, ints or booleans | -

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *body:* Array of instance UUIDs  
- *message:* Error message in case of an error 

**CURL Example**
```shell
 curl -X POST "http://anka.controller.net/api/v1/vm" -H "Content-Type: application/json" \
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

**CURL Example**
```shell
 curl -X PUT "http://anka.controller.net/api/v1/vm?id=c0f36a87-41d9-44a8-66e1-6c5afae15b80" -H "Content-Type: application/json" \
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


**CURL Example**
```shell
curl -X DELETE "http://anka.controller.net/api/v1/vm" -H "Content-Type: application/json" -d '{"id": "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce"}' 

{
   "status" : "OK",
   "message" : ""
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

**CURL Example**
```shell
# List all VMs

curl  "http://anka.controller.net/api/v1/vm" -H "Content-Type: application/json" 

{
   "status" : "OK",
   "body" : [
      {
         "instance_id" : "04b7ca7a-945c-4bdc-5123-68b2e4c8ad13",
         "name": "My VM",
         "external_id": "ly79930",
         "vm" : {
            "anka_registry" : "",
            "ts" : "2019-12-25T16:06:20.681609561Z",
            "progress" : 0,
            "vminfo" : {
               "ip" : "192.168.64.4",
               "status" : "running",
               "cpu_cores" : 2,
               "name" : "mgmtManaged-build_vm3-MacPro-02.local-1574781776830830000",
               "port_forwarding" : [
                  {
                     "Name" : "ssh",
                     "protocol" : "tcp",
                     "host_port" : 10001,
                     "guest_port" : 22
                  }
               ],
               "ram" : "2G",
               "vnc_port" : 5902,
               "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
               "vnc_connection_string" : "vnc://:@207.254.51.229:5902",
               "host_ip" : "207.254.51.229",
               "uuid" : "381e5c9f-d453-48dc-87a4-af6a2a98e46f"
            },
            "tag" : "t1",
            "instance_state" : "Started",
            "cr_time" : "2019-11-26T15:22:52.221924626Z",
            "vmid" : "a29dcb72-f663-469d-9d83-8c4641b2e5dd",
            "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6"
         }
      },
      {
         "vm" : {
            "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
            "instance_state" : "Started",
            "cr_time" : "2019-12-25T16:06:04.658586219Z",
            "vmid" : "6b135004-0c89-43bb-b892-74796b8d266c",
            "vminfo" : {
               "status" : "running",
               "cpu_cores" : 2,
               "ip" : "",
               "name" : "mgmtManaged-mojave-w-java-1577289969489444000",
               "vnc_port" : 5903,
               "ram" : "2G",
               "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
               "uuid" : "4284cb1a-246c-4a2f-b773-ab18d6785957",
               "host_ip" : "207.254.51.229",
               "vnc_connection_string" : "vnc://:@207.254.51.229:5903"
            },
            "tag" : "teamcity",
            "anka_registry" : "",
            "ts" : "2019-12-25T16:06:20.717627395Z",
            "progress" : 0
         },
         "instance_id" : "6d5b7632-6ace-433a-44d9-e0c25f56706b"
      },
      {
         "vm" : {
            "tag" : "teamcity",
            "vminfo" : {
               "ip" : "",
               "tag" : "teamcity",
               "status" : "running",
               "cpu_cores" : 2,
               "name" : "mojave-w-java-1577289976968997000",
               "vnc_port" : 5904,
               "ram" : "2G",
               "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
               "host_ip" : "207.254.51.229",
               "vnc_connection_string" : "vnc://:admin@207.254.51.229:5904",
               "uuid" : "bfd4106c-2a88-462f-a545-50f832b7a0f7"
            },
            "progress" : 1,
            "anka_registry" : "",
            "ts" : "2019-12-25T16:06:21.163835272Z",
            "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
            "cr_time" : "2019-12-25T16:06:04.763922869Z",
            "vmid" : "6b135004-0c89-43bb-b892-74796b8d266c",
            "instance_state" : "Started"
         },
         "instance_id" : "9c623cc1-e489-4d02-4559-0c76492bec9f"
      }
   ],
   "message" : ""
}

# Show specific VM

curl "http://anka.controller.net/api/v1/vm?id=04b7ca7a-945c-4bdc-5123-68b2e4c8ad13" -H "Content-Type: application/json" 

{
   "message" : "",
   "status" : "OK",
   "body" : {
      "progress" : 0,
      "vminfo" : {
         "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
         "cpu_cores" : 2,
         "ip" : "192.168.64.4",
         "status" : "running",
         "uuid" : "381e5c9f-d453-48dc-87a4-af6a2a98e46f",
         "port_forwarding" : [
            {
               "host_port" : 10001,
               "Name" : "ssh",
               "protocol" : "tcp",
               "guest_port" : 22
            }
         ],
         "vnc_connection_string" : "vnc://:@207.254.51.229:5902",
         "host_ip" : "207.254.51.229",
         "ram" : "2G",
         "name" : "mgmtManaged-build_vm3-MacPro-02.local-1574781776830830000",
         "vnc_port" : 5902
      },
      "vmid" : "a29dcb72-f663-469d-9d83-8c4641b2e5dd",
      "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
      "anka_registry" : "",
      "ts" : "2019-12-26T08:57:48.392701636Z",
      "instance_state" : "Started",
      "cr_time" : "2019-11-26T15:22:52.221924626Z",
      "tag" : "t1"
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

**CURL Example**  
```shell
# List all Nodes

curl "http://anka.controller.net/api/v1/node" -H "Content-Type: application/json"

{
   "body" : [
      {
         "ip_address" : "207.254.51.229",
         "cpu_count" : 12,
         "capacity" : 6,
         "node_name" : "MacPro-02.local",
         "vm_count" : 1,
         "usb_devices" : null,
         "vcpu_count" : 2,
         "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
         "cpu_util" : 0.074900396,
         "vram" : 2048,
         "ram_util" : 0.03125,
         "state" : "Active",
         "vcpu_override" : 0,
         "anka_version" : {
            "build" : "112",
            "product" : "Anka Build Enterpriseplus",
            "version" : "2.1.2",
            "license" : "com.veertu.anka.entplus"
         },
         "free_disk_space" : 155624185856,
         "anka_disk_usage" : 38274388000,
         "disk_size" : 250135076864,
         "ram" : 64,
         "ram_override" : 0,
         "capacity_mode" : "number"
      }
   ],
   "status" : "OK",
   "message" : ""
}

# Show specific Node

curl  "http://anka.controller.net/api/v1/node?id=f8707005-4630-4c9c-8403-c9c5964097f6" -H "Content-Type: application/json" 

{
   "message" : "",
   "status" : "OK",
   "body" : [
      {
         "cpu_count" : 12,
         "vcpu_override" : 0,
         "capacity" : 6,
         "usb_devices" : null,
         "ram" : 64,
         "ram_util" : 0.03125,
         "state" : "Active",
         "ip_address" : "207.254.51.229",
         "vm_count" : 1,
         "vram" : 2048,
         "anka_version" : {
            "license" : "",
            "build" : "",
            "product" : "",
            "version" : ""
         },
         "capacity_mode" : "",
         "free_disk_space" : 155624185856,
         "anka_disk_usage" : 38274388000,
         "disk_size" : 250135076864,
         "cpu_util" : 0.08308069,
         "node_name" : "MacPro-02.local",
         "node_id" : "f8707005-4630-4c9c-8403-c9c5964097f6",
         "ram_override" : 0,
         "vcpu_count" : 2
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

**CURL Example**
```shell
curl -X POST "http://anka.controller.net/api/v1/node/config" -H "Content-Type: application/json" \
 -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6", "name": "MacPro1", "max_vm_count": 6}' 

{
   "status":"OK",
   "message":""
}
```

### Delete Node.

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

**CURL Example**
```shell
curl -X DELETE "http://anka.controller.net/api/v1/node" -H "Content-Type: application/json" \
 -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6"}' 

{
   "status":"OK",
   "message":""
}
```

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

**CURL Example**  
```shell
# List example

curl "http://anka.controller.net/api/v1/registry/vm" 

{
   "message" : "",
   "body" : [
      {
         "id" : "0bf1a7e8-be95-43d9-a0c8-68c6aed0f2dd",
         "name" : "jenkins-slave",
         "size" : 16427892736
      },
      {
         "id" : "1820b42d-6581-46af-bf42-f64caa1e9633",
         "name" : "catalina",
         "size" : 20643704832
      },
      {
         "id" : "2fa0f10e-e91e-4665-8d42-00a39b9707de",
         "name" : "Catalina-Xcode-11",
         "size" : 17834520576
      },
      {
         "name" : "CachedBuidMojave",
         "id" : "36fc63bb-6841-4528-9480-a9c44dc2740d",
         "size" : 15040651264
      },
      {
         "name" : "android-2",
         "id" : "59adf1a8-c239-11e8-821d-c4b301c47c6b",
         "size" : 19698943312
      }
   ],
   "status" : "OK"
}


# Get Single Template 

curl "http://anka.controller.net/api/v1/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" 

{
   "message" : "",
   "body" : {
      "name" : "android-2",
      "id" : "00510971-5c37-4a60-a9c6-ea185397d9b4",
      "versions" : [
         {
            "config_file" : "00510971-5c37-4a60-a9c6-ea185397d9b4.yaml",
            "state_files" : [
               "c19ba955c706475e9aeade79f174a925.ank"
            ],
            "number" : 0,
            "size" : 16634568704,
            "description" : "",
            "nvram" : "nvram",
            "images" : [
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
            "tag" : "t1"
         }
      ]
   },
   "status" : "OK"
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

**CURL Example**
```shell
curl -X DELETE "http://anka.controller.net/api/v1/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" 

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

**CURL Example**
```shell
# Delete latest version

curl -X DELETE  "http://anka.controller.net/api/v1/registry/revert?id=00510971-5c37-4a60-a9c6-ea185397d9b4"

{
   "body" : null,
   "message" : "",
   "status" : "OK"
}

# Revert to the first version of the template

curl -X DELETE  "http://anka.controller.net/api/v1/registry/revert?id=a3cc47f0-3a73-11e9-b515-c4b301c47c6b&number=0" 

{
   "status" : "OK",
   "body" : null,
   "message" : ""
}

# Revert to a specific Tag


curl -X DELETE  "http://anka.controller.net/api/v1/registry/revert?id=a3cc47f0-3a73-11e9-b515-c4b301c47c6b&tag=p120190904183122" 

{
   "status" : "OK",
   "body" : null,
   "message" : ""
}


```

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

**CURL Example**
```shell
curl -X POST "http://anka.controller.net/api/v1/registry/vm/distribute" -d '{"template_id": "eaef3af8-cb54-4c3e-baf9-839053472f15"}' 

{
   "body" : {
      "request_id" : "74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5"
   },
   "message" : "",
   "status" : "OK"
}


```

### Get distribution status 

**Description:** Get the status of a requested distribution  
**Path:** /api/v1/registry/vm/distribute  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return the Distribution with that ID. 


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

**CURL Example**
```shell
# List all distribution requests

curl  "http://anka.controller.net/api/v1/registry/vm/distribute" 

{
   "message" : "",
   "body" : [
      {
         "request" : {
            "time_requested" : "2019-12-26T17:48:50.380949005+02:00",
            "template_id" : "eaef3af8-cb54-4c3e-baf9-839053472f15",
            "request_id" : "74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5"
         },
         "distribute_status" : {
            "64230242-321c-442a-bd96-d87edd0943a3" : {
               "progress" : 0,
               "status" : false
            }
         }
      }
   ],
   "status" : "OK"
}

# Get a specific distribution request

curl  "http://anka.controller.net/api/v1/registry/vm/distribute?id=74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5" 

{
   "status" : "OK",
   "body" : {
      "distribute_status" : {
         "64230242-321c-442a-bd96-d87edd0943a3" : {
            "progress" : 0,
            "status" : false
         }
      },
      "request" : {
         "request_id" : "74efc824-2fcb-4e07-5e7d-7f9cc98e0ee5",
         "template_id" : "eaef3af8-cb54-4c3e-baf9-839053472f15",
         "time_requested" : "2019-12-26T17:48:50.380949005+02:00"
      }
   },
   "message" : ""
}



```

### Save Template Image

**Description:** Create a "Save Image" request. Save a running VM instance to the Registry as a new Tag or Template.  
**Path:** /api/v1/image  
**Method:** POST  
**Required Body Parameters:**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Id of the instance to save.

 **Optional Body Parameters:**   

 Parameter          | Type    | Description              | Default
 ---                | ---     |   ---                    | ---
 target_vm_id       | string  | The template to save the VM as. This should be empty if creating a new Image Template | -
 new_template_name  | string  | Create a new Image Template from the running instance, using this name. Required if target_vm_id not supplied | -
 tag                | string  | The tag name to use for the new tag | -
 description        | string  | The description to use for the new tag | -
 suspend            | bool    | If true, suspends the vm before pushing | true 
 script             | string  | Script to be executed before the instance is stopped/suspended, encoded as a base64 string. | -
 revert_before_push | bool    | If `target_vm_id` is not empty, revert the latest tag of the template or tag specified in `revert_tag` | false
 revert_tag         | string  | Revert this specific tag. In case the tag does not exist, revert operation does not take place. | -
 do_suspend_sanity_test | bool | If suspend is true, perform a suspend sanity test before pushing the VM to the registry | false

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body:* The created request id 
 - *message:* Error message in case of an error 

**CURL Example**
```shell
curl -X POST "http://anka.controller.net/api/v1/image" -H "Content-Type: application/json" -d '{"id": "cfc3cafd-d326-459d-7b3b-3c41b1a3efb7", "target_vm_id": "cb1473f2-1f0a-413c-a376-236bfd7d718f", "tag": "my-new-vm-1901", "revert_before_push": true, "revert_tag": "latest-vm-1801"}' 

{
   "status" : "OK",
   "body" : {
      "request_id" : "cc55f7dd-5280-4120-461c-9b0ef9b40131"
   },
   "message" : ""
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
- *request* - An object that represents the request
- *error* - Error message if status is error

**CURL Example**
```shell
# List all requests

curl "http://anka.controller.net/api/v1/image" 

{
   "body" : [
      {
         "id" : "cc55f7dd-5280-4120-461c-9b0ef9b40131",
         "status" : "pending",
         "request" : {
            "Suspend" : true,
            "InstanceID" : "cfc3cafd-d326-459d-7b3b-3c41b1a3efb7",
            "RequestId" : "cc55f7dd-5280-4120-461c-9b0ef9b40131",
            "RegistryAddr" : "http://192.168.1.108:8089",
            "NewTemplateName" : "",
            "Priority" : 0,
            "Description" : "",
            "Timestamp" : 1577364097,
            "NodeID" : "64230242-321c-442a-bd96-d87edd0943a3",
            "RevertTag" : "latest-vm-1801",
            "Tag" : "my-new-vm-1901",
            "TemplateID" : "cb1473f2-1f0a-413c-a376-236bfd7d718f",
            "Script" : "",
            "RevertBeforePush" : true,
            "VMID" : "578a3be9-ad37-49c3-99fc-59c41b79dc59",
            "DoSuspendTest" : false
         },
         "error" : null
      }
   ],
   "message" : "",
   "status" : "OK"
}

# Get specific request

curl "http://anka.controller.net/api/v1/image?id=cc55f7dd-5280-4120-461c-9b0ef9b40131" 

{
   "status" : "OK",
   "message" : "",
   "body" : {
      "status" : "pending",
      "request" : {
         "Tag" : "my-new-vm-1901",
         "Script" : "",
         "Timestamp" : 1577364097,
         "Priority" : 0,
         "RevertBeforePush" : true,
         "RequestId" : "cc55f7dd-5280-4120-461c-9b0ef9b40131",
         "VMID" : "578a3be9-ad37-49c3-99fc-59c41b79dc59",
         "RevertTag" : "latest-vm-1801",
         "TemplateID" : "cb1473f2-1f0a-413c-a376-236bfd7d718f",
         "NewTemplateName" : "",
         "Suspend" : true,
         "RegistryAddr" : "http://192.168.1.108:8089",
         "DoSuspendTest" : false,
         "NodeID" : "64230242-321c-442a-bd96-d87edd0943a3",
         "InstanceID" : "cfc3cafd-d326-459d-7b3b-3c41b1a3efb7",
         "Description" : ""
      },
      "error" : null,
      "id" : "cc55f7dd-5280-4120-461c-9b0ef9b40131"
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

**CURL Example**
```shell
 curl -X POST "http://anka.controller.net/api/v1/group" -d '{"name": "GreateNodes", "description": "best of my macs"}'

{
   "status" : "OK",
   "message" : "",
   "body" : {
      "fallback_group_id" : null,
      "name" : "GreateNodes",
      "description" : "best of my macs",
      "id" : "89a66167-62b1-42bb-5a0b-ff667906b8f5"
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

**CURL Example**
```shell
curl "http://anka.controller.net/api/v1/group" 

{
   "message" : "",
   "body" : [
      {
         "fallback_group_id" : null,
         "description" : "best of my macs",
         "id" : "89a66167-62b1-42bb-5a0b-ff667906b8f5",
         "name" : "GreateNodes"
      }
   ],
   "status" : "OK"
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

**CURL Example**
```shell
curl -X PUT "http://anka.controller.net/api/v1/group?id=89a66167-62b1-42bb-5a0b-ff667906b8f5" -d '{"name": "Creme-de-la-nodes", "description": "A selected group of my favorite computers"}'
{
   "message" : "",
   "status" : "OK"
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

**CURL Example**
```shell
curl -X DELETE "http://anka.controller.net/api/v1/group?id=89a66167-62b1-42bb-5a0b-ff667906b8f5" 

{
   "message" : "",
   "status" : "OK"
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
 node_ids  | string array | List of Nodes to add to the specified Groups. | []

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error 

**CURL Example**
```shell
curl -X POST "http://anka.controller.net/api/v1/node/group" -d '{"group_ids": ["e41d4b47-4c10-4264-5519-2d52af568bdd"], "node_ids": ["64230242-321c-442a-bd96-d87edd0943a3"]}'

{
   "status" : "OK",
   "message" : ""
}

```

### Remove nodes from a group


**Description:** Remove one or more Nodes from one or more Groups.  
**Path:** /api/v1/node/group  
**Method:** DELETE  

**Required Body Parameters:**   

 Parameter | Type         | Description       | Default
 ---       | ---          |   ---             | ---
 group_ids | string array | List of group ids to add the nodes to. | []
 node_ids  | string array | List of Nodes to add to the specified Groups. | []

**Returns:**  
- *status:* Operation Result (OK|FAIL)  
- *message:* Error message in case of an error 

**CURL Example**
```shell
curl -X DELETE "http://anka.controller.net/api/v1/node/group" -d '{"group_ids": ["e41d4b47-4c10-4264-5519-2d52af568bdd"], "node_ids": ["64230242-321c-442a-bd96-d87edd0943a3"]}'

{
   "status" : "OK",
   "message" : ""
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

**CURL Example**
```shell
curl "http://anka.controller.net/api/v1/usb" 

{
   "message" : "",
   "body" : {
      "iphone7": {
         "available": 3,
         "busy": 1
      },
      "tablet": {
         "available": 0,
         "busy": 2
      }
   },
   "status" : "OK"
}


```


