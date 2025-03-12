---
date: 2024-12-10T01:00:00-00:00
title: "Anka Virtualization 3.7.0"
---

## Anka Click Scripts Feed

Anka Click Scripts are now updated from a feed, allowing you to update them without installing a new Anka CLI version.

We have two feeds available:

- The default production feed: https://downloads.veertu.com/click-scripts/v1/feed.json
- The edge feed: https://downloads.veertu.com/edge-click-scripts/v1/feed.json

You can switch between feeds using the `anka config feed_url` command.

```
❯ anka config feed_url https://downloads.veertu.com/edge-click-scripts/v1/feed.json
```

You can disable the feed entirely by setting the feed url to an empty string.

## Advanced list filtering & output

You can now filter the output of `anka list` by status, name, and even by a specific match using `=`.
```
❯ anka list --field name,status=stopped
+-----------------------------+---------+
| name                        | status  |
+-----------------------------+---------+
| 15.1.1-xcode-16_16.1_16.2RC | stopped |
+-----------------------------+---------+
| 15.3.2                      | stopped |
+-----------------------------+---------+
```

```
❯ anka list --field name=15.3,status
+--------+---------+
| name   | status  |
+--------+---------+
| 15.3.2 | stopped |
+--------+---------+

❯ anka list --field name=15,status
+-----------------------------+---------+
| name                        | status  |
+-----------------------------+---------+
| 15.1.1-xcode-16_16.1_16.2RC | stopped |
+-----------------------------+---------+
| 15.3.2                      | stopped |
+-----------------------------+---------+
```

You can also add more columns/values to the output.

```
❯ anka list --field name,status,uuid,access_date,ip,vnc_port
+-----------------------------+---------+--------------------------------------+----------------------+--------------+----------+
| name                        | status  | uuid                                 | access_date          | ip           | vnc_port |
+-----------------------------+---------+--------------------------------------+----------------------+--------------+----------+
| 15.1.1-xcode-16_16.1_16.2RC | stopped | 6f1e83ad-70d2-4d87-ac0c-99a3e9b449cf | Feb 28 09:49:49 2025 |              | 0        |
+-----------------------------+---------+--------------------------------------+----------------------+--------------+----------+
| 15.3.2                      | stopped | 70351937-917d-4e13-8ee8-e58c37509a73 | Mar 12 16:27:52 2025 |              | 0        |
+-----------------------------+---------+--------------------------------------+----------------------+--------------+----------+
| testVM                      | running | 39f9c267-1563-4c85-9cb7-51ef5eeecf5f | Mar 12 16:28:19 2025 | 192.168.70.9 | 5900     |
+-----------------------------+---------+--------------------------------------+----------------------+--------------+----------+
```

```
❯ anka -j list --field name,status,uuid,access_date,ip,vnc_port | jq
{
  "status": "OK",
  "body": [
    {
      "name": "15.1.1-xcode-16_16.1_16.2RC",
      "status": "stopped",
      "uuid": "6f1e83ad-70d2-4d87-ac0c-99a3e9b449cf",
      "access_date": "2025-02-28T14:49:49Z",
      "ip": "",
      "vnc_port": 0
    },
    {
      "name": "15.3.2",
      "status": "stopped",
      "uuid": "70351937-917d-4e13-8ee8-e58c37509a73",
      "access_date": "2025-03-12T20:27:52Z",
      "ip": "",
      "vnc_port": 0
    },
    {
      "name": "testVM",
      "status": "running",
      "uuid": "39f9c267-1563-4c85-9cb7-51ef5eeecf5f",
      "access_date": "2025-03-12T20:29:08Z",
      "ip": "192.168.70.9",
      "vnc_port": 5900
    }
  ]
}
```
