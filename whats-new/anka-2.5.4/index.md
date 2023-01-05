---
date: 2021-11-23T01:00:00-00:00
title: "Anka Virtualization 2.5.4"
---

### Ability to find the last time a Template was used

Starting in 2.5.4, you can now find the last time a VM/template was used on an Anka Node. This is great if you're looking to automate the clean up of older templates from nodes when disk usage is high. It will update on start, create, stop, and clone.

```bash
❯ anka --machine-readable show 12.0.1 |jq
{
  "status": "OK",
  "body": {
    "uuid": "ea663a61-0e5c-4419-8194-697104fb693a",
    "name": "12.0.1",
. . .
    "access_date": "2021-11-12T10:29:24Z"
  }
}

❯ anka clone 12.0.1 test
VM created successfully with uuid: c9aa200f-808e-4daf-b3d0-32add7c1fe2c

❯ anka --machine-readable show 12.0.1 |jq
{
  "status": "OK",
  "body": {
    "uuid": "ea663a61-0e5c-4419-8194-697104fb693a",
    "name": "12.0.1",
. . .
    "access_date": "2021-11-15T18:00:41Z"
  }
}

❯ anka --machine-readable list -f uuid -f name -f access_date | jq
{
  "status": "OK",
  "body": [
    {
      "uuid": "ea663a61-0e5c-4419-8194-697104fb693a",
      "name": "12.0.1",
      "access_date": "2021-11-15T18:00:41Z"
    },
    {
      "uuid": "7c446d89-8ae3-44c1-a0d4-78b7be4ea3f7",
      "name": "master_vm",
      "access_date": "2021-11-09T17:05:34Z"
    },
    {
      "uuid": "c9aa200f-808e-4daf-b3d0-32add7c1fe2c",
      "name": "test",
      "access_date": "2021-11-15T18:00:41Z"
    },
    {
      "uuid": "da3e53cd-f3bc-44d3-ba84-57ed37ae24ec",
      "name": "test-mont",
      "access_date": "2021-11-12T18:00:10Z"
    }
  ]
}
```

### Local tagging happens after the registry is tested for availability

We now check the registry status/availability before creating a local tag. This should prevent situations where a local tag was created prematurely and then blocked subsequent commands (requiring either a manual deletion of the local tag, or a force push).
