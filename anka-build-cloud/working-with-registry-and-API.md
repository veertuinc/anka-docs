---
date: 2019-12-12T00:00:00-00:00
title: "Working with the Registry and API"
linkTitle: "Working with the Registry and API"
weight: 5
description: >
  How to work with the Anka Registry and API
---

Anka registry provides an easy way to store, version, and distribute macOS VMs that are used for CI and development. Once you've completed creation and setup of your VMs, use [`anka registry`]({{< relref "anka-virtualization-cli/command-line-reference.md#registry" >}}) command to work with the registry.  

Store your build and test VM templates in the registry.  
Then, you can pull them, modify them and push them again with a different tag to the Registry. This way you can maintain versions. Pull specific VM template and tag to a different machine running Anka package.

Registry operations are available through Anka Client command line and through Registry REST APIs.

## Working with registry using Anka CLI

View all of the available registry commands by running `anka registry --help`.

{{< include file="_partials/anka-virtualization-cli/command-line-reference/registry/_index.md" >}}

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

## REST API


### Status

```bash
❯ curl -s http://anka.registry:8089/registry/status                                                    
{"status":"OK","body":{"status":"Running","version":"1.13.0-24e848a5"},"message":""}
```

### List VMS v1 (Deprecated)

**Description:** List all VMs stored in the Registry.  
**Path:** /registry/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return a specific Template. Passing this parameter will show more information about the Template's tags.


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  List of VM objects. If id supplied, also returns list of tags ("versions").. 
 - *message:* Error message in case of an error 

**Template Object format**
 - *{UUID}:* *{Template Name}*

**CURL Example**  
```shell
# List example

❯ curl -s http://anka.registry:8089/registry/vm | jq
{
  "status": "OK",
  "body": [
    {
      "5d1b40b9-7e68-4807-a290-c59c66e926b4": "12.4-gitlab"
    },
    {
      "b6e96ed0-6d0f-4152-a97c-944553b3c35b": "12.4-openjdk-11.0.14.1"
    },
    {
      "c0847bc9-5d2d-4dbc-ba6a-240f7ff08032": "12.4-openjdk-11.0.14.1-jenkins"
    },
    {
      "c12ccfa5-8757-411e-9505-128190e9854e": "12.4"
    }
  ],
  "message": ""
}

# Get Single Template 

❯ curl -s http://anka.registry:8089/registry/vm\?id\=c12ccfa5-8757-411e-9505-128190e9854e | jq
{
  "status": "OK",
  "body": {
    "id": "c12ccfa5-8757-411e-9505-128190e9854e",
    "name": "12.4",
    "versions": [
      {
        "number": 0,
        "tag": "vanilla",
        "config_file": "c12ccfa5-8757-411e-9505-128190e9854e.yaml",
        "nvram": "nvram",
        "images": [
          "113eb088112f4b0c9d28197590058301.ank"
        ],
        "state_files": [],
        "description": "",
        "state_file": "",
        "size": 19490295808,
        "arch": ""
      },
      {
        "number": 1,
        "tag": "vanilla+port-forward-22",
        "config_file": "c12ccfa5-8757-411e-9505-128190e9854e.yaml",
        "nvram": "nvram",
        "images": [
          "113eb088112f4b0c9d28197590058301.ank"
        ],
        "state_files": [],
        "description": "",
        "state_file": "",
        "size": 19490295808,
        "arch": ""
      },
      {
        "number": 2,
        "tag": "vanilla+port-forward-22+brew-git",
        "config_file": "c12ccfa5-8757-411e-9505-128190e9854e.yaml",
        "nvram": "nvram",
        "images": [
          "c92b6575b4ed40bb825ad6e0903519dc.ank",
          "113eb088112f4b0c9d28197590058301.ank"
        ],
        "state_files": [],
        "description": "",
        "state_file": "",
        "size": 22706692096,
        "arch": ""
      }
    ],
    "size": 22706692096,
    "arch": "amd64"
  },
  "message": ""
}

```

### List VMS v2 

> **Note**   
> This is a new API introduced in version 1.7.0  

**Description:** List all VMs stored in the Registry.  
**Path:** /registry/v2/vm  
**Method:** GET  
**Optional Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | Return a specific Template. Passing this parameter will show more information about the Template's tags 


 **Returns:** 
 - *Status:* Operation Result (OK|FAIL)  
 - *Body*:  List of VM objects. If id supplied, also returns list of tags ("versions")..
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
 - *creation_date:* The date and time the version was created in Unix epoch time.
 - *last_pull:* The date and time the version was last pulled in Unix epoch time.
 - *last_push:* The date and time the version was last pushed in Unix epoch time.
 - *arch:* The architecture of the version ("arm64" or "amd64")

**CURL Example**  
```shell
# List example

curl -s http://anka.registry:8089/registry/v2/vm | jq                          {
  "status": "OK",
  "body": [
    {
      "id": "c12ccfa5-8757-411e-9505-128190e9854e",
      "name": "11.6.0",
      "size": 21024776192
    }
  ],
  "message": ""
}


# Get Single Template 

curl "http://localhost:8089/registry/v2/vm?id=2db3814c-2913-42b9-ae42-c33f1e6124d4" |jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1447  100  1447    0     0  1180k      0 --:--:-- --:--:-- --:--:-- 1413k
{
  "status": "OK",
  "message": "",
  "body": {
    "id": "2db3814c-2913-42b9-ae42-c33f1e6124d4",
    "name": "sequoia",
    "versions": [
      {
        "number": 0,
        "tag": "suspended",
        "config_file": "2db3814c-2913-42b9-ae42-c33f1e6124d4.yaml",
        "nvram": "nvram",
        "images": [
          "b16512b62a07485e853e6c67426094c0.ank",
          "3b1d13f4a360444788a046ac98d7925d.ank"
        ],
        "state_files": [
          "16b5fd251f724d699df853e7718d750a.ank"
        ],
        "description": "",
        "state_file": "16b5fd251f724d699df853e7718d750a.ank",
        "size": 26937917952,
        "arch": "",
        "last_pull": 1741354557,
        "last_push": 1741354557,
        "creation_date": 1736162801
      },
      {
        "number": 1,
        "tag": "ssh",
        "config_file": "2db3814c-2913-42b9-ae42-c33f1e6124d4.yaml",
        "nvram": "nvram",
        "images": [
          "b16512b62a07485e853e6c67426094c0.ank",
          "3b1d13f4a360444788a046ac98d7925d.ank"
        ],
        "state_files": [
          "27a1c0f5c902420395ca32db43aa97be.ank",
          "16b5fd251f724d699df853e7718d750a.ank"
        ],
        "description": "",
        "state_file": "27a1c0f5c902420395ca32db43aa97be.ank",
        "size": 26937922052,
        "arch": "",
        "last_pull": 1741354557,
        "last_push": 1741354557,
        "creation_date": 1739188070
      },
      {
        "number": 2,
        "tag": "java17",
        "config_file": "2db3814c-2913-42b9-ae42-c33f1e6124d4.yaml",
        "nvram": "nvram",
        "images": [
          "808ed9baffd04a888e157de653a9fd7a.ank",
          "b16512b62a07485e853e6c67426094c0.ank",
          "3b1d13f4a360444788a046ac98d7925d.ank"
        ],
        "state_files": [],
        "description": "",
        "state_file": "",
        "size": 28217180160,
        "arch": "",
        "last_pull": 1742211405,
        "last_push": 1741354557,
        "creation_date": 1739189692
      }
    ],
    "size": 28221379076,
    "arch": "arm64",
    "last_pull": 1742211405,
    "last_push": 1741354557
  }
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

{{< hint warning >}}
Reverting is a potentially dangerous operation. It will revert all tags which came AFTER the one you're targeting AND the target tag itself. We recommend a multi-template-single-tag approach, providing the flexibility to delete a template when it's no longer needed and not lose other layers.
{{< /hint >}}

**Description:** Reverts a Template's Tags. Will default to reverting the latest Tag if no Tag or version is specified. If specified, the target tag and all Tags that came after it will be deleted.
**Path:** /registry/revert  
**Method:** DELETE  
**Required Query Parameters**  

 Parameter | Type   | Description 
 ---       |   ---  |          ---
 id        | string | The Template id. 

**Optional Data Parameters**  

 Parameter | Type   | Description     | Default
 ---       |   ---  |          --- | ---
 tag       | string | The Tag to revert. Newer versions will also be deleted. | Latest 
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

❯ curl -X DELETE  "http://anka.registry:8089/registry/revert?id=aff190a4-9871-4fb7-b909-73742f0fbdbe" -d '{"tag": "vanilla+port-forward-22+brew-git"}'
{"status":"OK","body":null,"message":""}

```

### Set Template Arch 

**Description:** Set the template's arch manually.  
**Path:** /registry/vm/arch  
**Method:** PUT

**Required Data Parameters**  

 Parameter | Type   | Description | Default
 ---       |   ---  |          --- | ---
 id        | string | The Template id. | -
 arch      | string | The architecture ("arm64" or "amd64") | -

 **Returns:**
 - *Status:* Operation Result (OK|FAIL) 
 - *message:* Error message in case of an error

**CURL Example**

 ```bash

❯ curl -s http://10.8.1.10:8089/registry/vm\?id\=0e1660cb-632f-4748-8a0d-ce82ae6bcd25 | jq
{
  "status": "OK",
  "body": {
    "id": "0e1660cb-632f-4748-8a0d-ce82ae6bcd25",
    "name": "12.1.0",
    ("versions").[
      {
        "number": 0,
        "tag": "vanilla+addons",
        "config_file": "0e1660cb-632f-4748-8a0d-ce82ae6bcd25.yaml",
        "nvram": "nvram",
        "images": [
          "7180b240cfc8434299212b8fc06c3f04.ank"
        ],
        "state_files": [],
        "description": "",
        "state_file": "",
        "size": 19425398784,
        "arch": ""
      }
    ],
    "size": 19425398784,
    "arch": "amd64"
  },
  "message": ""
}

❯ curl -X PUT http://10.8.1.10:8089/registry/vm/arch -d '{"id": "0e1660cb-632f-4748-8a0d-ce82ae6bcd25", "arch": "arm64"}'
{"status":"OK","body":null,"message":""}


❯ curl -s http://10.8.1.10:8089/registry/vm\?id\=0e1660cb-632f-4748-8a0d-ce82ae6bcd25 | jq
{
  "status": "OK",
  "body": {
    "id": "0e1660cb-632f-4748-8a0d-ce82ae6bcd25",
    "name": "12.1.0",
    ("versions").[
      {
        "number": 0,
        "tag": "vanilla+addons",
        "config_file": "0e1660cb-632f-4748-8a0d-ce82ae6bcd25.yaml",
        "nvram": "nvram",
        "images": [
          "7180b240cfc8434299212b8fc06c3f04.ank"
        ],
        "state_files": [],
        "description": "",
        "state_file": "",
        "size": 19425398784,
        "arch": ""
      }
    ],
    "size": 19425398784,
    "arch": "arm64"
  },
  "message": ""
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
❯ curl -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/v1/apikey | jq
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
| groups | string array | List of group names for the key (required) | - |
| publicKey | string | If supplied, it is expected to be in PKIX ASN.1 DER form (optional) | - |

> If no publicKey is passed in, we will generate and store it on the server.

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `message`: Error message in case of an error
- `body`: JSON returned from API

```bash
❯ curl -X POST -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/v1/apikey -d '{ "id": "nathan", "ttl": 86400, "groups": ["g1"] }'
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
❯ curl -X PUT -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/v1/apikey -d '{ "id": "nathan", "groups": ["test1","test3"] }'
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
❯ curl -X DELETE -sH "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/v1/apikey -d '{ "id": "nathan" }'
{"status":"OK","message":""}
```

## Miscellaneous

### Disk Info

- **Description:** Display Disk usage information.

- **Path:** /registry/disk

- **Method:** GET

**Returns:**
- `status`: Operation Result (OK|FAIL)
- `body`: Free and Total disk space for the registry
- `message`: Error message in case of an error

```bash
❯ curl https://anka.registry:8089/registry/disk
{"status":"OK","body":{"free":335356690432,"total":1000240963584},"message":""}
```
