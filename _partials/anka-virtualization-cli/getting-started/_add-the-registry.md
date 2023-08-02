---
---

{{< hint warning >}}
This is not needed on all Nodes connecting to the Controller. Most users have a "builder machine" that has the registry connection and is used to create and push VM Templates/Tags.
{{< /hint >}}

{{< hint warning >}}
Assuming you haven't changed the default port configuration, your Registry is serving requests on port `8089`.
{{< /hint >}}

We now need to configure the Registry on this machine so we can push/upload the local VM Template so other machines connected to the Anka Build Cloud. Uploading the Template to the Registry makes it possible to download and run it from other nodes. 

```shell
sudo anka registry add <registry name> http://<ip>:8089
```

Verify the configuration:

```shell
sudo anka registry list-repos
++
++

<registry name you set> (default)

+--------+------------------+
| host   | <the ip you set> |
+--------+------------------+
| scheme | http             |
+--------+------------------+
| port   | 8089             |
+--------+------------------+
```

Then, confirm the registry list command shows your VM Template:

```shell
sudo anka registry list
```
