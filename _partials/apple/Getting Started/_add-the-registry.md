We now need to configure the Registry on this machine so we can push/upload the local VM Template we created earlier. Uploading the Template to the Registry makes it possible to download and run it from other nodes.

_Assuming you haven't changed the default port configuration, your Registry is serving requests on port `8089`._

```shell
sudo anka registry add <registry name here> http://<ip>:8089
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
Then, confirm the registry list command doesn't throw any failures:
```shell
sudo anka registry list
```