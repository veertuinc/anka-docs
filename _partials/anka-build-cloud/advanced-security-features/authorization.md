---
---
Authorization allows you to control access to specific actions/endpoints of the API, and even specific resources like Nodes and Templates in your Controller and Registry. It has two features that are important to understand:

1. **Permission Groups**
1. **Resource Permissions**

You can think of **Permission Groups** (or just "Groups") as the wrapper for all permissions. Therefore, **Resource Permissions** are a dependent on **Permission Groups**. You can enable **Permission Groups** without **Resource Permissions**, but cannot have it the other way.

To enable Authorization features, you'll need to ensure that both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` ENVs are set in your Controller & Registry config and you're familiar with one of the existing [Authentication]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) methods. You then log into your Controller using the Root Token (or as you'll see later on, a credential/group that can modify permissions).

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