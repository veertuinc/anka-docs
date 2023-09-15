---
---

Authorization allows you to control access to specific actions/endpoints of the API and even specific Resources like Nodes and Templates in your Controller and Registry. It has four parts to it that are important to understand:

- **Groups**
    - **Permissions**
    - **Resources**
      - **Permissions**

**Groups** are the wrappers for all Permissions and Resources. You attach a Groups to a [Authentication]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) credential to enable certain access.

**Permissions** are given to allow a credential access to perform a specific action, like listing or creating a VM Instances.

**Resources** limit which Nodes and Templates the credential can see, start VMs on, as well as the Permissions for each specific Resource. As an example, these Resource Permissions allow an admin to prevent a specific Group from deleting a Node from the cloud, yet allow changes to its config, and more.

Permissions and Resources are dependent on Group. However, Resource Management is optional. If disabled, all Resources (and Resource Permissions) are available to all credentials.

After a Group is created, you'll assign it to a specific credential. The credential can have one or more Groups attached and it's important you consider how much access for each group you need to provide for the use-case and security.

In this guide we will start with the most common use-case of Sharing Nodes, then explain how to isolate Nodes per team (similar to how the older Node Groups worked).

#### Prerequisites

To enable Authorization features, you'll need to ensure that both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` ENVs are set in your Controller & Registry config and you're familiar with one of the existing [Authentication]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) methods. Then, turn Authorization on Controller and Registry configuration:

- `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` works for both combined (macOS) and standalone (docker) packages.
- `ANKA_ENABLE_REGISTRY_AUTHORIZATION` is for the macOS combined (controller + registry in one) package only.
- `ANKA_ENABLE_AUTHORIZATION` is only for the standalone (macOS or docker) registry packages.

You then log into your Controller using the Root Token and access the `https://<controller address>/#/permission-groups` page.

{{< hint warning >}}
Do not confuse Node Groups with Permission Groups.
{{< /hint >}}

{{< hint warning >}}
License Considerations: These features are only available for Enterprise tier licenses. 
- Enterprise license customers cannot control authorization and each authentication credential will have full access to the system.
- However, Enterprise Plus customers will be able to use **Permission Groups** and Resources to have more fine-grained control over access.
{{< /hint >}}

{{< include file="_partials/anka-build-cloud/advanced-security-features/permission-groups.md" >}}

{{< include file="_partials/anka-build-cloud/advanced-security-features/resource-permissions.md" >}}