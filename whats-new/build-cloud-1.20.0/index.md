---
date: 2021-11-08T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.20.0"
---

### View available/pulled VM Templates on a Node from Controller API

```shell
curl "http://anka.controller/api/v1/node" -H "Content-Type: application/json" | jq
{
   "body": [
      {
        "node_name": "MacPro-02.local",
        "node_id": "f8707005-4630-4c9c-8403-c9c5964097f6",
        "cpu_util": 0.074900396,
        "vram": 2048,
        "ram_util": 0.03125,
        "state": "Active",

        . . . 

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
```

### Delete VM Templates on Node from Controller API

#### [API DOCUMENTATION]({{< relref "Anka Build Cloud/working-with-controller-and-api.md#delete-template-from-nodes" >}})

```shell
❯ sudo anka registry pull ea663a61-0e5c-4419-8194-697104fb693a
Downloading files  [####################################]  100%
VM pulled successfully with uuid: ea663a61-0e5c-4419-8194-697104fb693a

❯ sudo anka list | grep ea663a61-0e5c-4419-8194-697104fb693a
| 12.0.1 (vanilla+port-forward-22+brew-git) | ea663a61-0e5c-4419-8194-697104fb693a | Oct 25 18:21:58 2021 | stopped   |

❯ curl -s -X DELETE "http://anka.controller/api/v1/registry/vm/remove?id=ea663a61-0e5c-4419-8194-697104fb693a" | jq
{
  "status": "OK",
  "message": "",
  "body": {
    "requests_sent": 1,
    "node_ids": [
      "3c101836-9540-4733-9482-604d0c5fbe30"
    ]
  }
}

❯ tail -10 /var/log/veertu/anka_agent.INFO
I1108 16:53:03.618043   60538 node_task.go:74] queue id: ready/40/1636408383525209000_3631e927-ed35-4759-5c2a-14bfdfd38756
I1108 16:53:14.088676   60538 node_task.go:48] performing node task 79d24405-6dcd-4030-52be-604943375bc9
I1108 16:53:14.088696   60538 node_task.go:165] got NodeConfigRequest: Version 2
I1108 16:53:14.088702   60538 node_config.go:11] Setting config version: 2  config: {2 2 0xc00055d900 [{beb5835d-18e4-4c66-631a-2646f40ed273 gitlab-test-group-env  <nil>}] <nil> number 0 0 http://anka.registry:8089 true}
I1108 16:53:14.088724   60538 node_config.go:31] Setting config version: 2
I1108 16:53:14.118665   60538 node_task.go:74] queue id: ready/40/1636408394088956000_1079496f-8901-4ffd-465c-4e58f11856f3
I1108 16:56:21.148750   60538 node_task.go:48] performing node task b5f868cb-4d2e-4e24-4464-e0a624c8b017
I1108 16:56:21.148812   60538 node_task.go:114] requestId:b5f868cb-4d2e-4e24-4464-e0a624c8b017, got stop vm request for ea663a61-0e5c-4419-8194-697104fb693a
I1108 16:56:22.177877   60538 node_task.go:119] requestId:b5f868cb-4d2e-4e24-4464-e0a624c8b017, succeeded . instance id
I1108 16:56:22.178994   60538 node_task.go:74] queue id: ready/20/1636408582178213000_10742a2a-513e-44a4-5459-3e9d976f3a39
```
