```shell
sudo ankacluster join http://<ip>
Password:
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success
```

- Replace `<ip>` with the IP of the machine hosting your controller:
- If you changed the default port for the controller from 80, you'll need to use the new port at the end of the IP. Otherwise, leave it off.

The command may hang for a few moments and then display `Cluster join success`. Please report any errors you find to support@veertu.com.