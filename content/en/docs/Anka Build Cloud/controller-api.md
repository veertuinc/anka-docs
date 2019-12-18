
---
date: 2019-12-12T00:00:00-00:00
title: "Controller API"
linkTitle: "Controller API"
weight: 7
description: >
  How to work with the Anka Controller REST API.
---


Use the REST APIs to integrate Anka Build cloud with your CI system(If there is no plugin/integration available).

## Provision and start VMs (Start VM  instance will start N count of instances of a particular type across the node cluster)

***Note*** Group_id, priority and USB_device is only available if you are running Enterprise and higher tier of Anka Build.

```html
url= "/api/v1/vm", 
method="POST"
body="VM UUID, tag, version, count, node_id, startup_script, group_id, priority, usb_device"  
(node_id, count, tag, version, startup_script, group_id, priority, usb_device are optional. If tag is not provided, the latest version will be used, otherwise whatever tag is specified will be pulled on the nodes. By default count is 1. If node_id is not specified the VM could be scheduled on any node)
startup_script is base64 encoded string of the script to be executed after the instance is started
Group_id lets you choose on which group the vm will run on
Priority lets you give the vm priority over other vms, most urgent priority is 1 and the default priority is 1000
Returns: Operation Result, Array of instance UUIDs
Example Body: {"vmid":"226d946b-2467-11e7-84b7-a860b60fd826", â€œcountâ€=10, startup_script=â€IyEvYmluL2Jhc2gNCmV4cG9ydCBBPSJhYWEiDQoNCg==â€}

```

## Terminate VM
```html
url= "/api/v1/vm", 
method="DELETE"
body=Instance ID
Example Body:  {"id":"7ed72625-ee2e-4195-606a-dd04a274b9c3"}
Returns: Operation result

```

## List the VM instances
```html
url= "/api/v1/vm"
method="GET"
body=none
Returns: Operation result, List of initiated VM instance UUIDs 
```

## Show information for an instance
```html
url= "/api/v1/vm?id={instance_id}"
method="GET"
body=none
Returns: Operation result, State, VM and connectivity info

```

## List all build nodes joined to the controller
```html
url= "/api/v1/node"
method="GET"
body=none
Returns: Operation result, List of nodes IDs

```

## Show information for a build node
```html
url= "/api/v1/node?id={node_id}"
method="GET"
body=none
Returns: Operation result, State and Info of the node and available USB devices
```

## Update node configuration
```html
url= "/api/v1/node/config"
method="POST"
body: JSON
Keys:
‘node_id’:  string, the node id (required)
‘capacity-mode’: string, the new capacity mode (number|resource)
‘vcpu_override’: int, override the default vcpu scheduling limit
‘ram_override’: int, override the default ram scheduling limit
Example Request Body:
{"node_id": "524c3781-0035-45fe-8d98-4d6bad7d79a7", "capacity_mode": "resource"}
```

## Delete/remove a build node from controller database/portal.

***Note*** This does not remove the node from the cluster. Use `ankacluster disjoin` on the node to disjoin the node from the cluster. Then, execute delete/remove node API call to remove it permanently from the controller database.
```html
url= "/api/v1/node"
method="DELETE"
body=node_id ID
Example Body:  {"node_id":"7ed72625-ee2e-4195-606a-dd04a274b9c3"}
Returns: Operation result

```
## create request to save image to the registry connected to the controller
```html
url= "/api/v1/image"
method="POST"
body= 
{
	"id": (string) the instance id to save
            "target_vm_id": (string) the target template to save the image to (optional,  required if new_template_name not supplied)
             "new_template_name": (string) save the image as a template with this name (optional, required if target_vm_id not supplied)
             "tag": (string) the tag name to use for the push 
             "description": (string) the description to use for the push (optional)
             "suspend": (boolean) if true, suspends the vm before push (optional, defaults to true)
             "script"
             "revert_before_push": (boolean) when supplying 'target_vm_id' revert the latest tag of the template or tag specified in 'revert_tag' (optional, defaults to false)
             "revert_tag": (string) revert this specific tag, if tag does not exist revert does not happen (optional)
             "do_suspend_sanity_test": (boolean) if suspend is true, perform a suspend sanity test before pushing to the registry (optional)

}
Returns: json 
{
	“status”: “OK”,
	“body”: save_request_id ,
	“message”: error message if not “OK”
}
```
To get status on save image request
```html
Path: api/v1/image
Method: GET
Query Parameters: “id” (returns only the single request)
Returns: json
Save image request format: 
{
        “id”: the request’s id,
        “status”: pending/done/error,
        “request”: an object that represents the request,
        “error”: error message if status is error
}
If id supplied:
{
	“status”: “OK”,
	“body”:  single ‘Save image request’,
	“message”: error message if not ‘OK’
}
If id not supplied:
{
	“status”: “OK”,
	“body”:  [] list of ‘Save image requests’,
	“message”: error message if not ‘OK’
}
```
## Manage ankacluster join flag which enables/disables node sending anka agent logs to the controller
```html
Path: /api/v1/node/config
Method: POST
Body: json
{
   “node_id”: the node’s id,
   "disable_central_logging": boolean (true to stop, false to start/resume) 
}
Returns:
{
    “status”: “OK”,
    “message”: “”  error message if not ‘OK’
}

Curl example:
curl -X POST "192.168.1.100:80/api/v1/node/config" -d '{"node_id": "9759b4ba-b0bf-4346-b72a-d8b5ff794873", "disable_central_logging": false}'
```

***Note*** - Group and USB management APIs are only available in Enterprise and higher Anka Build Tier.

## Get a list of all groups defined
```html
List Groups
url= "/api/v1/group"
method=â€GET"
body=none 
Returns: Operation result, List of groups
```

## Create a new Group
```html
url= "/api/v1/group"
method="POST"
body="Name, Description" 
Example Body: {"name": "My awesome group", "description": "my favorite nodes"}
Returns: Operation result, The new group
```

## Update a Group
```html
url= "/api/v1/group?id={group_id}"
method=â€PUT"
body=â€Name, Descriptionâ€
 Example Body: {"name": "My other awesome group", "description": "my second favorite nodes"}
Returns: Operation result
```

## Delete a Group
```html
url= "/api/v1/group?id={group_id}"
method=â€DELETE"
body=none 
Returns: Operation result
```

## Add nodes to a Group
```html
Add Nodes To Groups 
url= "/api/v1/node/group"
method="POST"
body="Groupd Ids,  Node Ids" 
Example Body: {"group_ids": ["a2a833a1-e44c-4247-57d1-6cd06b0fd040", "47f0a57e-9b14-4d80-4f5c-3284f2c85e0d"], "node_ids": ["c6493f47-7106-4cb9-88be-1f5cf2af7a72"]}
Returns: Operation result
```

## Remove nodes from a group
```html
url= "/api/v1/node/group"
method="DELETE"
body="Groupd Ids,  Node Ids" 
Example Body: {"group_ids": ["a2a833a1-e44c-4247-57d1-6cd06b0fd040", "47f0a57e-9b14-4d80-4f5c-3284f2c85e0d"], "node_ids": ["c6493f47-7106-4cb9-88be-1f5cf2af7a72"]}
Returns: Operation result
```
## Get a list of all USB devices attached to the cloud
```html
List all USB devices
url= "/api/v1/usb"
method=â€GET"
body=none 
Returns: Operation result, List of all USB devices attached across all the nodes
```
