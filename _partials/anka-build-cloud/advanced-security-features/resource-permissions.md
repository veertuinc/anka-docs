---
---

#### Resource Permissions

{{< hint warning >}}
Node Groups are disabled while Resource Management is enabled.
{{< /hint >}}

{{< hint warning >}}
Resource Management "Groups" differ from the Node Groups you may already be familiar with.
{{< /hint >}}

{{< hint warning >}}
Node Groups are disabled while Resource Management is enabled.
{{< /hint >}}

The Resource Permissions feature is enabled by setting **ANKA_ENABLE_RESOURCE_MANAGEMENT** to **true** in your configuration. Once enabled, it unlocks the Resources tab under **/permission-groups**.

Within Resources you will be able to, depending on the Component selected, add specific Nodes or Templates to the Group.

Once added, you will be able to set the Resource Permissions, allowing you to limit the Group to certain actions that can be performed against the Resource. An example of this is allowing the iOS team/group to distribute specific Templates to specific Nodes, but not create VM Instances or delete the Node.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/resources-basic.png" >}}

In the previous section on Permissions, we joined a Node using a credential with the Permission Group of `node` attached. This Group only has permissions to perform the minimum required actions to run as an Anka Node and communicate with the Controller. We did not add Resources to it though (even though we technically could) so we could have a team specific Groups with specific resources assigned.

**It's important to understand that a credential, like a UAK or a certificate, should only ever be used by a specific user and client.** You wouldn't ever want to share the Node credential with a team for example. Create a second credential for that team, then in order for the Node to be able to access the team's credential, add the team's Group to the Node credential.

Under the **Resources** tab, you'll see the available resources you can add granular permissions for. For example, the Controller Component will have Node Resources you can control and the Registry, Template Resources.

{{< hint info >}}
These permissions must be added through the root user (using the root token).
{{< /hint >}}



In the above example, you'll see that the **node** Permission Group has access to perform very specific actions for the node Veertu.local. For example, any authentication method (UAK, Certs, etc) with the **devops** group attached will be unable to Remove the node, but will be able to Create Instances on it.

##### Answers to Frequently Asked Questions

- The Nodes joined to the Controller must have Permissions to access the Template being used to start a VM.
- Node Groups differ from Permissions Groups and are disabled when this feature is enabled.
- Save Image requests can only target Instances that belong to a group the user has access to.
