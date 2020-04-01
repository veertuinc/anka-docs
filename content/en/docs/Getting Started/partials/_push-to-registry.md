
### Link the Registry

We now need to configure the registry on this machine so we can push/upload the local VM Template we created earlier. Uploading the Template to the Registry makes it possible to download and run it from other nodes.

_Assuming you haven't changed the default port configuration, your Registry is serving requests on port `8089`._

Execute the `registry add` command to add your Registry to Anka CLI:
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

sudo anka registry list  # this command should succeed

```

### Push the VM to the Registry
```shell
sudo anka registry push 10.14.6 -t base
```


