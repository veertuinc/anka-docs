
Great! now that we have our Anka Controller & Registry up and running let's add Nodes!

## Step 3. Link the Anka CLI Node to the Controller

> ***NOTE***  
> Perform the following steps on the Node where you created your first VM Template.

### Add the Registry

We now need to configure the registry on this machine so we can push/upload the local VM Template we created earlier. Uploading the Template to the Registry makes it possible to download and run it from other nodes.

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

### Push the VM to the Registry
```shell
sudo anka registry push mojave-base -t base
```

After the push is finished you should see your new Template in the "Templates" section of the controller UI.

![Your first template](/images/getting-started/push-template.png)

### Join to the Controller & Registry

```shell
sudo ankacluster join http://<ip>
Password:
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success
```

- Replace `<ip>` with the IP of the machine hosting your controller:
- If you changed the default port for the controller (from 80), you'll need to put it on the end of the IP. Otherwise, leave it off and it will automatically use 80.

The command may hang for a few moments before printing `"Cluster join success"`. In that period of time, any errors that occur will be printed. Please report any errors you find to support@veertu.com.

> ***NOTE***  
> Repeat this process on other Nodes that you want to join (Anka CLI needs to be installed).

## Step 4. Start a VM instance using the Controller UI

1. Go to your Controller dashboard and click on the **Instances** tab:

    ![your instances view](/images/getting-started/instances.png)

2. Click on **Create Instance(s)**, the **Create New Instances** view will open:

    ![new instances view](/images/getting-started/new-instance.png)

3. Select the VM Template and click **Start**. the **Create New Instances** view will close and return you to the **Instances** view. You should now see the Instance in a `Scheduling` or `Pulling` **State**:
    ![a scheduling instance](/images/getting-started/scheduling.png)

4. After the `Scheduling` and `Pulling` finishes, the VM will start on one of the Nodes and show a `Started` **State**:

    ![a started instance](/images/getting-started/started-vm.png)

You can now confirm the Instance is running from inside the Node:

- JSON output is available for scripting/automation using `anka --machine-readable`
    
```
sudo anka --machine-readable list | jq
{
  "status": "OK",
  "body": [
    {
      "status": "suspended",
      "name": "mojave-base",
      "stop_date": "2020-04-01T21:30:59.798697Z",
      "creation_date": "2020-04-01T00:00:13.656296Z",
      "version": "base",
      "uuid": "10c720eb-dcce-46f7-baa3-28bacef0ec0f"
    },
    {
      "status": "running",
      "name": "mgmtManaged-mojave-base-1585776660490226000",
      "stop_date": "2020-04-01T21:36:11.742662Z",
      "creation_date": "2020-04-01T21:31:01.055250Z",
      "version": "",
      "uuid": "dcbeb319-421a-4d30-8466-194eb7fa5f75"
    }
  ],
  "message": ""
}
```