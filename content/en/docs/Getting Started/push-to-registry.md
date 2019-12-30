
### Configure the Registry
We will configure the registry on this machine, so we can push the VM to it. Saving the VM on the Registry makes it possible to download and run it from other nodes. 
When executing the commands replace `192.168.1.10` with IP of the machine running your Registry. Assuming you haven't changed the default port configuration, your Registry is serving requests on port `8089`.

Execute the `registry add` command to add your Registry to Anka CLI.
```shell
anka registry add MyRegistry http://192.168.1.10:8089
```
Verify the configuration:  
```shell
anka registry list  # this command should succeed

anka registry list-repos
++
++

MyRegistry (default)

+--------+---------------+
| host   | 192.168.1.10 |
+--------+---------------+
| scheme | http          |
+--------+---------------+
| port   | 8089          |
+--------+---------------+


```

### Push the VM 
Assuming your VM is called `mojave-base`, execute the `registry push` command:  
```shell
anka registry push mojave-base -t base
```


