
Great! now that we have our Anka Controller & Registry up and running let's add Nodes!

## Step 3. Link the Anka CLI Node to the Controller

> ***NOTE***  
> Perform the following steps on the Node where you created your first VM Template.

{{< include file="shared/content/en/docs/Getting Started/partials/_push-to-registry.md" >}}

After the push is finished you should see your new Template in the "templates" section of the controller UI.

![Your first template](/images/getting-started/push-template.png)

{{< include file="shared/content/en/docs/Getting Started/partials/_join-node-to-controller.md" >}}

> ***NOTE***  
> Repeat this process on other Nodes that you want to join (Anka CLI needs to be installed).

## Step 4. Start a VM instance using the Controller UI

{{< include file="shared/content/en/docs/Getting Started/partials/_start-vm-from-controller.md" >}}

You can now confirm the Instance is running from inside the Node:

- JSON output is available for scripting/automation using `anka --machine-readable`
    
```
sudo anka --machine-readable list | jq
{
  "status": "OK",
  "body": [
    {
      "status": "suspended",
      "name": "10.14.6",
      "stop_date": "2020-04-01T21:30:59.798697Z",
      "creation_date": "2020-04-01T00:00:13.656296Z",
      "version": "base",
      "uuid": "10c720eb-dcce-46f7-baa3-28bacef0ec0f"
    },
    {
      "status": "running",
      "name": "mgmtManaged-10.14.6-1585776660490226000",
      "stop_date": "2020-04-01T21:36:11.742662Z",
      "creation_date": "2020-04-01T21:31:01.055250Z",
      "version": "",
      "uuid": "dcbeb319-421a-4d30-8466-194eb7fa5f75"
    }
  ],
  "message": ""
}
```

## What next?

- Browse the [Anka CLI Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).  
- Connect your cloud to a [CI server]({{< relref "docs/Anka Build Cloud/CI Plugins/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "docs/Anka Build Cloud/controller-api.md">}}).  
- Learn how to work with [USB devices]({{< relref "docs/Anka Build Cloud/using-real-devices-attached-to-anka-vms.md">}})