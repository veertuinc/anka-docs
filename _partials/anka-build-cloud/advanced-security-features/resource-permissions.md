---
---

{{< hint warning >}}
Resource Management "Groups" differ from the Node Groups you may already be familiar with.
{{< /hint >}}

{{< hint warning >}}
Node Groups are disabled while Resource Management is enabled.
{{< /hint >}}

The Resource Permissions feature is enabled by setting **ANKA_ENABLE_RESOURCE_MANAGEMENT** to **true** in your configuration. Once enabled, it unlocks the Resources tab under **/permission-groups**.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/resources-basic.png" >}}

Under the **Resources** tab, you'll see the available resources you can add granular permissions for. For example, the Controller Component will have Node Resources you can control and the Registry, Template Resources.

{{< hint info >}}
These permissions must be added through the root user (using the root token).
{{< /hint >}}

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/resources-controller-node-example.png" >}}

In the above example, you'll see that the **devops** Permission Group has access to perform very specific actions for the node Veertu.local. For example, any authentication method (UAK, Certs, etc) with the **devops** group attached will be unable to Remove the node, but will be able to Create Instances on it.
