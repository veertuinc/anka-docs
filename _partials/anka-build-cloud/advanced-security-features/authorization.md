---
---

Authorization allows you to control access to specific actions/endpoints of the API and even specific Resources like Nodes and Templates in your Controller and Registry. It has four parts to it that are important to understand:

- **Component** (Controller or Registry)
  - **Groups**
    - **Permissions**
    - **Resources**
      - **Permissions**

1. **Groups** are the wrappers for all Permissions and Resources. You attach a Groups to a [Authentication]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) credential to enable certain access.

    - **Group Permissions** are given to allow a credential access to perform a specific action, like listing or creating a VM Instances. Because these control general access to the API to make calls, it overrides any Resource Permissions.

1. **Resources** limit which Nodes and Templates the credential can control.

    - **Resource Permissions** are given to a Resource, further limiting the Group's access per Resource. As an example, these Resource Permissions allow an admin to prevent a specific Group from deleting a Node from the cloud, yet allow changes to its config, and more. It's critical to understand that if the Groups Permission "Instances > Start" is not checked, the Resource Permissions for the Group allowing Create Instance won't matter as the credential has no access to make a Start VM API call.
    - Resource Management, which enables Resource control, is optional. If disabled, all Resources (and Resource Permissions) are available to all credentials.

After a Group is created, you'll assign it to a specific credential. The credential can have one or more Groups attached and it's important you consider how much access for each Group you need to provide for your use-case and security requirements.

In this guide we will start with the most common use-case of sharing Node between multiple teams (Groups), then explain how to isolate Nodes per team (similar to how the older Node Groups worked) later on.

#### Prerequisites

To enable Authorization features, you'll need to ensure:

1. That both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` ENVs are set in your Controller & Registry config. You're also familiar with one of the existing [Authentication]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) methods and have one set up.
1. **Groups + Group Permissions** are enabled by enabling Authorization. The following ENVs can be added to your config to achieve this:

    - `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` (boolean) works for both combined (macOS) and standalone (docker) packages.
    - `ANKA_ENABLE_REGISTRY_AUTHORIZATION` (boolean) is for the macOS combined (controller + registry in one) package only.
    - `ANKA_ENABLE_AUTHORIZATION` (boolean) is only for the standalone (macOS or docker) registry packages.

    This should expose the `https://<controller address>/#/permission-groups` page in your Controller.
1. **Resource Permissions** are enabled by setting **ANKA_ENABLE_RESOURCE_MANAGEMENT** to **true** in your configuration.


{{< hint warning >}}
License Considerations: These features are only available for Enterprise tier licenses.
- Enterprise license customers cannot control authorization and each authentication credential will have full access to the system.
- However, Enterprise Plus customers will be able to use **Permission Groups** and Resources to have more fine-grained control over access.
{{< /hint >}}

{{< include file="_partials/anka-build-cloud/advanced-security-features/permission-groups.md" >}}

{{< include file="_partials/anka-build-cloud/advanced-security-features/resource-permissions.md" >}}