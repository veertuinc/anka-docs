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

#### [API DOCUMENTATION]({{< relref "anka-build-cloud/working-with-controller-and-api.md#delete-template-from-nodes" >}})

{{< include file="_partials/anka-build-cloud/whatsnew/1.20.0/delete-template-from-nodes.md" >}}
