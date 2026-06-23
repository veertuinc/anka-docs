```shell
sudo anka registry -r http://anka.registry:8089 push 12.X -t base
```

{{< hint info >}}
In the example above, `-r {URL}` is used, but is not required if you've added the Registry using `anka registry add`.
{{< /hint >}}

After the push completes, you should see your new Template in the "Templates" section of the controller UI.

![Your first template]({{< siteurl >}}images/getting-started/push-template.png)

Alternatively, you can use OCI compliant registries. See [OCI Registry Support]({{< relref "whats-new/anka-3.9.0/index.md#oci-registry-support" >}}) for more details.