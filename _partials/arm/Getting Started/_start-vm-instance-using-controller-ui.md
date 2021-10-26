1. Go to your Controller dashboard and click on the **Instances** tab:

    ![your instances view](/anka/images/getting-started/instances.png)

2. Click on **Create Instance(s)**, and the **Create New Instances** view displays:

    ![new instances view](/anka/images/getting-started/new-instance.png)

3. Select the VM Template and click **Start**. The **Create New Instances** view closes and returns you to the **Instances** view. You should now see the Instance in a **Scheduling** or **Pulling** **State**:
    ![a scheduling instance](/anka/images/getting-started/scheduling.png)

4. After the **Scheduling** and **Pulling** finishes, the VM starts on one of the Nodes and shows a **Started** **State** in the Controller UI:

    ![a started instance](/anka/images/getting-started/started-vm.png)

You can now confirm the Instance is running from inside the Node:

- JSON output is available for scripting/automation using `anka --machine-readable`
    
```shell
sudo anka --machine-readable list | jq
{
  "status": "OK",
  "body": [
    {
      "status": "suspended",
      "name": "catalina",
      "stop_date": "2020-04-01T21:30:59.798697Z",
      "creation_date": "2020-04-01T00:00:13.656296Z",
      "version": "base",
      "uuid": "10c720eb-dcce-46f7-baa3-28bacef0ec0f"
    },
    {
      "status": "running",
      "name": "mgmtManaged-catalina-1585776660490226000",
      "stop_date": "2020-04-01T21:36:11.742662Z",
      "creation_date": "2020-04-01T21:31:01.055250Z",
      "version": "",
      "uuid": "dcbeb319-421a-4d30-8466-194eb7fa5f75"
    }
  ],
  "message": ""
}
```

> Timestamp format: https://www.ietf.org/rfc/rfc3339.txt