```shell
❯ anka list                        
+----------------------+--------------------------------------+---------------------+---------+
| name                 | uuid                                 | creation_date       | status  |
+----------------------+--------------------------------------+---------------------+---------+
| 12.1.0-arm (vanilla) | a6f24306-2af7-45ed-9d70-3a3c1ee7f03a | Jan 6 12:34:52 2022 | stopped |
+----------------------+--------------------------------------+---------------------+---------+
| test_vm1             | 565e47ce-a9f9-4ac8-81bc-645d48473de1 | Jan 6 12:34:52 2022 | stopped |
+----------------------+--------------------------------------+---------------------+---------+

❯ anka --machine-readable list | jq
{
  "status": "OK",
  "body": [
    {
      "name": "12.1.0-arm",
      "uuid": "a6f24306-2af7-45ed-9d70-3a3c1ee7f03a",
      "creation_date": "2022-01-06T12:34:52Z",
      "version": "vanilla",
      "status": "stopped"
    },
    {
      "name": "test_vm1",
      "uuid": "565e47ce-a9f9-4ac8-81bc-645d48473de1",
      "creation_date": "2022-01-06T12:34:52Z",
      "status": "stopped"
    }
  ]
}

❯ anka --machine-readable list --field name --field version | jq
{
  "status": "OK",
  "body": [
    {
      "name": "12.1.0-arm",
      "version": "vanilla"
    },
    {
      "name": "test_vm1"
    }
  ]
}
```