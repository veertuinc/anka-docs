
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

## Working with registry using Anka CLI

View all of the available registry commands by running `anka registry --help`.
```shell
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
```shell
anka registry add <givesomename> <registryURLwithport>
```  

You can add/define multiple registries to a node. The last one added is treated as current. To change current use the set command.  

### Set Current registry
```shell
anka registry set <previouslydefiniedname>
```  

### Push VM to the Registry 
```shell
anka registry push -d <description> -t <tag> vmname
```  

### Pull VM from the Registry 
```shell
anka registry pull -t <tag> vmname
```
### Pull VM from the Registry with Shrink 
```shell
anka registry pull -s -t <tag> vmname
```
For example, let's say you have V1, V2 tags in the Registry for a VM. You pull V2. Then, you pull V1 with -s flag. It will optimize the local disk space usage by deleting all V2 related tag/version files.  

### Push independent VM as new tag of existing VM in Registry 
```shell
anka registry push NEW_VM -v EXISTING_VM_IN_REGISTRY -t NEW_TAG
``` 
For example, let's say you have a VM1, with latest tag t2. Now, you want to push a completely independent VM2 as the next tag to VM1. You will use `anka registry push VM2 -v VM1 -t NEW_TAG`.

### List VMs in the Registry 
```shell
anka registry list
```

## REST API

### List VMS  

> **Note**   
> This is a new API introduced in version 1.7.0  
> Old path /registry/vm still works for backward compatibility.

**Description:** List all VMs stored in the Registry.  
**Path:** /registry/v2/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return a specific Template. Passing this parameter will show more information about the Template's tags 


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  List of VM objects. If id supplied, also returns list of versions.
 - *message:* Error message in case of an error 

**Template format**
 - *name:* Template's name
 - *id:* Template's id
 - *size:* Total size of all Template's files on the registry in bytes
 - *versions:* Array of Version objects. 

**Version format**
 - *tag:* The version's tag
 - *number:* Serial number of the version
 - *description:* The version's description
 - *images:* List of image file names
 - *state_files:* List of state file names (suspend images)
 - *config_file:* Name of the version's VM config file
 - *nvram:* Name of the VM nvram file
 - *size:* Total size of all version's files on registry in bytes.

**CURL Example**  
```shell
# List example

curl "http://anka.registry.net:8089/registry/vm" 

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
         "name" : "Mojave-base",
         "size" : 20643704832
      },
      {
         "id" : "2fa0f10e-e91e-4665-8d42-00a39b9707de",
         "name" : "Catalina-Xcode-11",
         "size" : 17834520576
      }
   ],
   "status" : "OK"
}

# Get Single Template 

curl "http://anka.registry.net:8089/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" 

{
   "message" : "",
   "body" : {
      "name" : "android-2",
      "id" : "00510971-5c37-4a60-a9c6-ea185397d9b4",
      "size" : 18795192320,
      "versions" : [
         {
            "config_file" : "00510971-5c37-4a60-a9c6-ea185397d9b4.yaml",
            "state_files" : [
               "c19ba955c706475e9aeade79f174a925.ank"
            ],
            "number" : 0,
            "size" : 18795192320
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


### Delete VM

**Description:** Delete a specific VM and all associated tags  
**Path:** /registry/vm  
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
curl -X DELETE "http://anka.registry.net:8089/registry/vm?id=00510971-5c37-4a60-a9c6-ea185397d9b4" 

{
   "status":"OK",
   "body": null,
   "message":""
}
```


### Get VM Configuration

**Description:**  Show vm configuration file for a specific version or tag (or latest if none specified) of a VM template  
**Path:**   /registry/vm/info  
**Method:** GET  
**Required Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return the VM with that ID. 

**Optional Query Parameters**  

 Parameter | Type   | Description | Default
 ---       |   ---  |          --- | ---
 tag       | string | The specific Tag to get info for | Latest 
 version   | int    | The number of the version to get info for, 0 indexed | Latest

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  VM configuration object
 - *message:* Error message in case of an error 

**Object format**
 - *name:* Name of the Template
 - *ram:* Amount of virtual RAM the VM has
 - *hard_drives:* Array of hard drives 
 - *usb:* USB info
 - *uuid:* Id of the vm
 - *network_cards:* A list of network cards, with port forwarding rules
 - *firmware:* VM's firmware
 - *state_file:* Name of state file (suspend image)
 - *nvram:* Path of nvram file
 - *version:* VM configuration version (internally used)
 - *cpu:* Number of virtual CPU cores
 - *display:* VM's display 

**CURL Example**
```shell
curl "http://anka.registry.net:8089/registry/vm/info?id=2fa0f10e-e91e-4665-8d42-00a39b9707de"

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


### Revert 

**Description:** Revert a VM to a certain Tag or version number. Delete the latest version if none is specified.  
**Path:** /registry/revert  
**Method:** DELETE  
**Required Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The Template id. 

**Optional Query Parameters**  

 Parameter | Type   | Description     | Default
 ---       |   ---  |          --- | ---
 tag       | string | The Tag to revert to. Newer versions will also be deleted | Latest 
 version   | int    | The number of the version to revert to, 0 indexed | Latest

 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *message:* Error message in case of an error 

**CURL Example**
```shell
# Delete latest version

curl -X DELETE  "http://anka.registry.net:8089/registry/revert?id=00510971-5c37-4a60-a9c6-ea185397d9b4"

{
   "body" : null,
   "message" : "",
   "status" : "OK"
}

# Revert to the first version of the template

curl -X DELETE  "http://anka.registry.net:8089/registry/revert?id=a3cc47f0-3a73-11e9-b515-c4b301c47c6b&number=0" 

{
   "status" : "OK",
   "body" : null,
   "message" : ""
}

# Revert to a specific Tag


curl -X DELETE  "http://anka.registry.net:8089/registry/revert?id=a3cc47f0-3a73-11e9-b515-c4b301c47c6b&tag=p120190904183122" 

{
   "status" : "OK",
   "body" : null,
   "message" : ""
}


```
