---
date: 2024-09-10T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.43.0"
---

### Last Push and Pull dates for Templates and Tags

The last push and pull dates for Templates and Tags are now displayed in the UI. These are helpful to see when a Template or Tag was last used and determine if the Template or Tag is stale and can be removed.

{{< imgwithlink src="images/whatsnew/1.43.0/templates-and-tags-last-push-pull.png" >}}

You can also find the times in the API (in epoch/unix seconds):

```
❯ curl -s http://anka.controller:8090/api/v1/registry/vm | jq
{
  "status": "OK",
  "message": "",
  "body": [
    {
      "id": "0222a870-0b8c-44b3-989c-775f5cd0785f",
      "name": "14.5-arm64-jre17.48.15",
      "size": 28046262272,
      "arch": "arm64",
      "last_pull": 1727209082,
      "last_push": 1721141574
    },
```

```
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

### The Controller's /api/v1/nodes now return the license expiration date

```
❯ curl -s http://anka.controller:8090/api/v1/node | jq
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
      "cpu_util": 0.30706337,
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
```