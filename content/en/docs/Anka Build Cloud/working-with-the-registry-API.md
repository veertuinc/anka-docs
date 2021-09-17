---
date: 2019-12-12T00:00:00-00:00
title: "Working with the Registry API"
linkTitle: "Working with the Registry API"
weight: 5
description: >
  How to work with the Anka Registry API
---

Anka registry provides an easy way to store, version, and distribute macOS VMs that are used for CI and development. Once you've completed creation and setup of your VMs, use [`anka registry`]({{< relref "docs/Anka Virtualization/command-reference.md#registry" >}}) command to work with the registry.  

Store your build and test VM templates in the registry.  
Then, you can pull them, modify them and push them again with a different tag to the Registry. This way you can maintain versions. Pull specific VM template and tag to a different machine running Anka package.

Registry operations are available through Anka Client command line and through Registry REST APIs.

## Working with the Registry using Anka CLI

View all of the available registry commands by running `anka registry --help`.

{{< include file="./shared/content/en/docs/Anka Virtualization/partials/registry/_index.md" >}}

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
anka registry push -d <description> -t <tag> {vmNameOrUUID}
```  

### Pull VM from the Registry 
```shell
anka registry pull -t <tag> {vmNameOrUUID}
```
### Pull VM from the Registry with Shrink 
```shell
anka registry pull -s -t <tag> {vmNameOrUUID}
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

# REST API


### Status

```bash
❯ curl -s http://anka.registry:8089/registry/status                                                    
{"status":"OK","body":{"status":"Running","version":"1.13.0-24e848a5"},"message":""}
```

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
         "name" : "catalina",
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

## User Key Management

### Get Key

- **Description:** Display information about api key\[s\]

- **Path:** /v1/apikey

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
❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/api/v1/apikey | jq
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


### Create Key

- **Description:** Create user key

- **Path:** /v1/apikey

- **Method:** POST

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | Set the id/name of the key (required) | - |
| ttl | int | Set the TTL (time to live) seconds of the key (required) | - |
| groups | string | Comma separated list of group names for the key (optional) | - |
| publicKey | string | If supplied, it is expected to be in PKIX ASN.1 DER form (optional) | - |

> If no publicKey is passed in, we will generate and store it on the server.

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error
- `body`: JSON returned from API

```bash
❯ curl -X POST -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/api/v1/apikey -d '{ "id": "developer3", "ttl": 86400 }'
{"status":"OK","message":"","body":"MIIEowIBAAKCAQEAxU4GRMjtyFnEw69v/HxCfaw1zj28r2u1TpsnmGgp/q56o/DLSjemVLfYu+wIUKtu3zByIgFv/fbOAKKXEgUUeFl7wObgHeKNT4XOZG2L7yASxUHgFBokkimYCtQgEWy6bj9im+9k5OEA/kBN47NwHoZrnX6Av19WrpdAPaNO10yvaJ3R1jitshZqq6qtwmtKGErAvTPsloDcBOb2ojMghQHOJIVLKyIWwJ/yOsZme88vwO2DZ4wFYb8urnIj9e9P+Rf9WUH4RJ4RBIjoWnvFXEeSu2MeBawNPP1xT+RDNS/By8FUGbs9XutRU2f6trXW0MhjDIKQqXu8xL/rkVMo4QIDAQABAoIBAC2WF3CxE+9kfkfQMwmdFCfBMDVUolHNivh8dKuL9Zf4bDGQowekz/Nm/tZ8dHdfBKDgqp4cwulkqrgO2OS6873SvvsU3NdKzud+vO8LOHepa2zUCZfbanOy1IQ7+qTMAE8Uqg3pQ+B19CrKZL . . .
```

### Update Key

- **Description:** Update an existing key

- **Path:** /v1/apikey

- **Method:** PUT

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | The id/name of the key (required) | - |
| ttl | int | The TTL (time to live) seconds for the key (optional) | - |
| groups | string | Comma separated list of group names for the key (optional) | - |

> You cannot change the publicKey.

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error

```bash
❯ curl -X PUT -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/api/v1/apikey -d '{ "id": "developer3", "groups": ["test1","test3"] }'
{"status":"OK","message":""}
```

### Delete Key

- **Description:** Delete an existing key

- **Path:** /v1/apikey

- **Method:** DELETE

**Query Parameters:**

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| id | string | The id/name of the key (required) | - |

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error

```bash
❯ curl -X DELETE -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/api/v1/apikey -d '{ "id": "developer3" }'
{"status":"OK","message":""}
```
