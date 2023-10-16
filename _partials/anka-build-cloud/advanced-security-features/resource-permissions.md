---
---

#### Resource Permissions

{{< hint warning >}}
Node Groups are disabled while Resource Management is enabled.
{{< /hint >}}

The Resource Permissions feature is enabled by setting **ANKA_ENABLE_RESOURCE_MANAGEMENT** to **true** in your configuration. Once enabled, it unlocks the Resources tab under **/permission-groups**.

Within Resources you will be able to, depending on the Component selected, add specific Nodes or Templates to the Group. This limits the Group to certain actions that can be performed against the Resource. An example of this is allowing the iOS team/group to distribute specific Templates to specific Nodes, but not create VM Instances or delete the Node.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/resources-basic.png" >}}

In the [previous section on Permissions](#permission-groups), we joined a Node using a credential with the Permission Group of `node` attached. This Group only has Permissions to perform the minimum required actions to run as an Anka Node and communicate with the Controller. We *did not* add Resources to it though (even though we technically could) so we can instead have team specific Groups. This is the first recommended method of grouping permissions: Each team gets only Resources assigned to their Group, and other Groups handle the ability to request certain information from and perform actions to the Controller and Registry.

{{< hint warning >}}
**It's important to understand that a single credential, like a [UAK]({{< relref "anka-build-cloud/Advanced Security Features/uak-tap-authentication.md" >}}) or a [Certificate]({{< relref "anka-build-cloud/Advanced Security Features/certificate-authentication.md" >}}), should only ever be used by a specific user and client.** You wouldn't ever want to share the Node credential with a team for example. Create a second credential for that team, then in order for the Node to be able to access the team's credential, add the team's Group to the Node credential.
{{< /hint >}}

1. Create two groups: `ios` and `instance-control`.

2. Under the `ios` Group > Resources, add the Template Resource you want this team to access. The bare minimum permissions are seen in the image below.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-ios-resources-template.png" >}}

2. Under the `instance-control` Group > Permissions, add all of the Instances Permissions like in the image below.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-instance-control-perms.png" >}}

2. Now create a *new credential* (not Group) named `service-user` and attach those two groups. We'll use [UAKs]({{< relref "anka-build-cloud/Advanced Security Features/uak-tap-authentication.md" >}}) for this example. There is no need for the `service-user` to have its own group as it will get the VM start and terminate permissions from `instance-control` group and Template/Node permissions from the `ios` group.

3. Attach the group `ios` to the `node` credential we created in the previous section and joined our node with so that that it can also collect information about the Template in order to start the VM Instance properly (it checks the download size of the template before pulling). Your UAK setup should look something like this image.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-uak-service-user-list.png" >}}

We can now use `service-user` in our CI/CD tools to communicate with the Controller when an `ios` team member triggers a job and that job requests a VM Instance to run the job commands inside of. When the `service-user` makes an API call to start a VM Instance, it will pass in the `ios` group, as that group has access to the template being targeted.

#### Node Groups

The method described above works well for sharing the same nodes amongst all teams in an organization. But what if you want to isolate specific nodes to specific teams? Node Groups are disabled when Resource Permissions are enabled, but that should make sense to you by now as Node access/permissions are now bound to a specific Group + a specific credential. There are are several other scenarios possible which we'll detail below.

{{< hint warning >}}
You cannot have a shared Node credential and also limit by Node Resources for specific teams.
{{< /hint >}}


##### Scenario 1: Team Specific Anka Nodes

This configuration allows isolating certain Nodes to certain teams. The service-user credential is necessary to make API calls. In the Controller > Instances page or the API the team member can now start VM Instances with the group `team1` (or 2, 3) and it will only start on the Nodes assigned to the team's group. This is due to the Resource Permission for the Node being attached to the team group.

###### High Level Overview

- Three Anka Nodes joined to the Controller, each with separate credentials.
- Three Node credentials used to join:
  - UAK: **team1-nodes** - Groups attached: **node,team1**
  - UAK: **team2-nodes** - Groups attached: **node,team2**
  - UAK: **team3-nodes** - Groups attached: **node,team3**
- Three "service user" credentials for teams to make API calls:
  - UAK: **team1-su** | Groups attached: **service-user,team1**
  - UAK: **team2-su** - Groups attached: **service-user,team2**
  - UAK: **team3-su** - Groups attached: **service-user,team3**
- Group `node` has
  - Permissions: Recommended Node permissions (see UI).
  - Resources:
- Group `service_user` has
  - Permissions: **Instances, Nodes, and Distribute.**
  - Resources:
- Group `team1/team2/team3` has
  - Permissions: None.
  - Resources: The specific Node (and other Resources) for the team.

##### Scenario 2: Shared Anka Nodes / Teams limited by VM Template Resource Permissions

This configuration allows all teams to share all Anka Node capacity, but only be able to start specific VMs from specific Templates Resources assigned to their team's Permission Group.

###### High Level Overview

- Three Anka Nodes joined to the Controller, each with the same credential.
- Three Node credentials used to join:
  - UAK: **nodes** - Groups attached: **node,team1,team2,team3**
  - UAK: **nodes** - Groups attached: **node,team1,team2,team3**
  - UAK: **nodes** - Groups attached: **node,team1,team2,team3**
- Three "service user" credentials for teams to make API calls:
  - UAK: **team1-su** | Groups attached: **service-user,team1**
  - UAK: **team2-su** - Groups attached: **service-user,team2**
  - UAK: **team3-su** - Groups attached: **service-user,team3**
- Group `node` has
  - Permissions: Recommended Node permissions (see UI).
  - Resources: None.
- Group `service_user` has
  - Permissions: **Instances, Nodes, and Distribute.**
  - Resources: None
- Group `team1/team2/team3` has
  - Permissions: None.
  - Resources: The specific Template Resources for that team. No Nodes.

<!-- Scenario 3: Shared node credential, full access
	- node1 with UAK node(node,all-templates)
	- node2 with UAK node(node,all-templates)
	- node3 with UAK node(node,all-templates)
	- UAK ios_su(service_user,all-templates)
	- UAK android_su(service_user,all-templates)
	*You don't need Resource Management at all for this* -->

##### Scenario 3: Team Specific Anka Nodes + Dynamic Nodes

This configuration allows teams to have specific nodes guaranteed, then admins to increase capacity by assigning the specific team's group to the nodes.

###### High Level Overview

- Four Anka Nodes joined to the Controller, each with separate credentials. Three get assign to specific teams, but the fourth can dynamically change to provide extra capacity to specific teams when needed.
- Four Node credentials used to join:
  - UAK: **team1-nodes** - Groups attached: **node,team1**
  - UAK: **team2-nodes** - Groups attached: **node,team2**
  - UAK: **team3-nodes** - Groups attached: **node,team3**
  - UAK: **dynamic-nodes** - Groups attached: **node**
    - Dynamically update this UAK's groups to add any team or teams that need the extra capacity
- Three "service user" credentials for teams to make API calls:
  - UAK: **team1-su** | Groups attached: **service-user,team1**
  - UAK: **team2-su** - Groups attached: **service-user,team2**
  - UAK: **team3-su** - Groups attached: **service-user,team3**
- Group `node` has
  - Permissions: Recommended Node permissions (see UI).
  - Resources:
- Group `service_user` has
  - Permissions: **Instances, Nodes, and Distribute.**
  - Resources:
- Group `team1/team2/team3` has
  - Permissions: None.
  - Resources: The specific Node (and other Resources) for the team.

##### Answers to Frequently Asked Questions

- The Nodes joined to the Controller must have Permissions to access the Template being used to start a VM.
- Node Groups differ from Permissions Groups and are disabled when this feature is enabled.
- Save Image requests can only target Instances that belong to a group the user has access to.
