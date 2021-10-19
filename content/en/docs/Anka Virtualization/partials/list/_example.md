```shell
❯ anka list
+---------------------+--------------------------------------+----------------------+---------+
| name                | uuid                                 | creation_date        | status  |
+---------------------+--------------------------------------+----------------------+---------+
| 12.0-beta-xcode13.1 | 61e1a370-89b4-435a-a816-8bfdf7e9fe5a | Oct 15 08:20:40 2021 | stopped |
+---------------------+--------------------------------------+----------------------+---------+
| 12.0-beta (v2)      | 26c18e20-f67a-4387-a7b7-236a277bb424 | Oct 15 08:20:40 2021 | stopped |
+---------------------+--------------------------------------+----------------------+---------+

❯ anka --machine-readable list | jq
{
  "status": "OK",
  "body": [
    {
      "name": "12.0-beta-xcode13.1",
      "uuid": "61e1a370-89b4-435a-a816-8bfdf7e9fe5a",
      "creation_date": "2021-10-16T07:20:40Z",
      "status": "stopped"
    },
    {
      "name": "12.0-beta",
      "uuid": "26c18e20-f67a-4387-a7b7-236a277bb424",
      "creation_date": "2021-10-16T07:20:40Z",
      "version": "v2",
      "status": "stopped"
    }
  ]
}

❯ anka --machine-readable list --field name --field version | jq
{
  "status": "OK",
  "body": [
    {
      "name": "12.0-beta-xcode13.1"
    },
    {
      "name": "12.0-beta",
      "version": "v2"
    }
  ]
}
```