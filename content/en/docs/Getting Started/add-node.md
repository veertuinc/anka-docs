

### Add a Node to the cloud

We'll use `192.168.1.10` as the IP for the machine running the Controller. Replace `192.168.1.10` with the real IP or host name of your machine.

Run the Anka Agent daemon:  
```shell
sudo ankacluster join http://192.168.1.10
Password:
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success
```
The command should hang for a few moments before printing `Cluster join success`. In that period of time if any errors occur they will be printed out.

