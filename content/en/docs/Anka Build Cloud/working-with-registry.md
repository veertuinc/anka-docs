
---
date: 2019-12-12T00:00:00-00:00
title: "Working with the Registry"
linkTitle: "Working with the Registry"
weight: 5
description: >
  How to work with the Anka Registry.
---

Anka registry provides an easy way to store, version, and distribute macOS VMs that are used for CI and development. Once you've completed creation and setup of your VMs, use `anka registry` command to work with the registry.  

Store your build and test VM templates in the registry.  
Then, you can pull them, modify them and push them again with a different tag to the Registry. This way you can maintain versions. Pull specific VM template and tag to a different machine running Anka package.

Registry operations are available through Anka Client command line and through Registry REST APIs.

## Working with registry from Anka Client

View all of the available registry commands by running `anka registry --help`.
```
anka registry --help
Usage: anka registry [OPTIONS] COMMAND [ARGS]...

  VMs registry

Options:
  -r, --remote TEXT           Use repository name instead of 'default'
  -a, --registry-path TEXT    Use repository URL instead of 'default'
  -c, -i, --cert, --pem PATH  Client certificate if required
  -k, --key PATH              Private key, if the client certificate doesn't contain one
  --cacert, --root-cert PATH  Alternate ca_bundle
  --insecure                  Skip TLS verification
  --help                      Show this message and exit.

Commands:
  add                  Add repository
  check-download-size  Returns size of the data to be downloaded
  delete               Forget repository
  describe             Shows VM description
  list                 List VMs in repository
  list-repos           List registry repositories
  pull                 Pull certain VM
  push                 Push VM version (tag) to repository
  set                  Set default registry


```
### Add registry to a Node/machine running Anka
`anka registry add <givesomename> <registryURLwithport>`  

You can add/define multiple registries to a node. The last one added is treated as current. To change current use the set command.  

### Set Current registry
`anka registry set <previouslydefiniedname>`  

### Push VM to the Registry 
`anka registry push -d <description> -t <tag> vmname`  

### Pull VM from the Registry 
`anka registry pull -t <tag> vmname`  

### Pull VM from the Registry with Shrink 
`anka registry pull -s -t <tag> vmname`. For example, let's say you have V1, V2 tags in the Registry for a VM. You pull V2. Then, you pull V1 with -s flag. It will optimize the local disk space usage by deleting all V2 related tag/version files.  

### Push independent VM as new tag of existing VM in Registry 
`anka registry push NEW_VM -v EXISTING_VM_IN_REGISTRY -t NEW_TAG`. For example, let's say you have a VM1, with latest tag t2. Now, you want to push a completely independent VM2 as the next tag to VM1. You will use `anka registry push VM2 -v VM1 -t NEW_TAG`.

### List VMs in the Registry 
`anka registry list`

## Working with Controller REST APIs for Registry operations  

***Note*** Execute these against Controller ip:port.

### To list all VM templates stored in the registry 

```html
url= "/api/v1/registry/vm"
method="GET"
body=none
Returns: Operation result, List of registry VM UUIDs and names
```

### To show information on a specific VM template from the registry 

```html
url= "/api/v1/registry/vm?id={vm_id}"
method="GET"
body=none
Returns: Operation result, VM UUID, name and tag list
```

### To show vm config file for a specific version or tag (or latest if none specified) of a VM template 

```html
url= "/api/v1/registry/vm?info"
method="GET"
Arguments:
id - string, the template id to get (required)
tag - string, get info for a specific tag (optional)
Version - int, get info for a specific version
If neither version or tag_name is supplied the latest version will be deleted
Return:Json, the vm config
Body Keys:
ram
hard_drives
usb
uuid
network_cards
firmware
state_file
name
nvram
version
cpu
display

Example Response
{
   "message" : "",
   "status" : "OK",
   "body" : {
      "nvram" : true,
      "state_file" : "668e52df4d454f0fbc14cc69a15c5006.ank",
      "display" : {
         "frame_buffer" : {
            "height" : 768,
            "pci_slot" : 29,
            "vnc_ip" : "0.0.0.0",
            "width" : 1024,
            "password" : "admin"
         }
      },
      "version" : 1,
      "usb" : {
         "pci_slot" : 7,
         "host" : 0,
         "tablet" : 1
      },
      "network_cards" : [
         {
            "mode" : "shared",
            "pci_slot" : 28,
            "mac_address" : "56:45:45:3c:db:02",
            "port_forwarding_rules" : [
               {
                  "host_port" : 0,
                  "protocol" : "tcp",
                  "rule_name" : "ssh1",
                  "guest_port" : 22,
                  "host_ip" : "0"
               }
            ]
         }
      ],
      "hard_drives" : [
         {
            "pci_slot" : 4,
            "controller" : "ablk",
            "file" : "239bb374545141ceb481d495ec01683e.ank"
         }
      ],
      "cpu" : {
         "cores" : 2
      },
      "name" : "appium-base",
      "ram" : "2G",
      "uuid" : "2fa0f10e-e91e-4665-8d42-00a39b9707de",
      "firmware" : {
         "type" : "uefi"
      }
   }
}
```

### To delete a specific VM template and all associated tags from the registry 

```html
url= "/api/v1/registry/vm?id={vm_id}"
method="DELETE"
body=none
Returns: Operation result
```
### To distribute a specific VM template to all build nodes. 

***Note*** Group_id is only available if you are running Enterprise tier of Anka Build.  

```html
url= "/api/v1/registry/vm/distribute"
method="POST"
body="VM Uuid,  tag, version, group id" 
group_id (optional) distributes the image to a specific group
Example Body: {"template_id":"226d946b-2467-11e7-84b7-a860b60fd826", "tag": "v1"}
Returns: Operation result, request id
```

### To get distribution status 

```html
url= "/api/v1/registry/vm/distribute?id={request_id}"
method="GET"
body=none 
Returns: Operation result, map of node id -> (distribution status, template id, tag, version, time)
```
### To delete a specific VM template-tag and all later tags from the registry.

***Note*** Use this against the IP:port of the registry.
```html
Url: "/registry/revert?id={vm_id}&tag_name={tobedeletedtag}"
Method: DELETE
Arguments:
vm_id - must
Version - the version number to delete, all newer versions will be deleted as well
Tag_name - specify tag name instead of numeric value
If neither version or tag_name is supplied the latest version will be deleted
Return:
{"status": "OK"} / {"status": "Error"}
```
