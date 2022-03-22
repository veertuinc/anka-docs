---
---
{{< hint info >}}
If you have any issues with this step, please see [our troubleshooting guide]({{< relref "intel/Troubleshooting/unable-to-join.md" >}}).
{{< /hint >}}

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

The command will hang for a few moments before displaying `Cluster join success`.
