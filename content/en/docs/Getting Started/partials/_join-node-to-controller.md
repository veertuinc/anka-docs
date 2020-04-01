

### Join to the Controller & Registry

Replace `<ip>` with the IP of the machine hosting your controller:
- If you changed the default port for the controller (from 80), you'll need to put it on the end of the IP. Otherwise, leave it off and it will automatically use 80.

```shell
sudo ankacluster join http://<ip>
Password:
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success
```

The command may hang for a few moments before printing `"Cluster join success"`. In that period of time, any errors that occur will be printed. Please report any errors you find to support@veertu.com.

