
You can limit the number of instances that a credential and/or group can use by creating a [Permission Group]({{< relref "anka-build-cloud/Advanced Security Features/authorization/index.md#groups--group-permissions" >}}) and setting the max instance quota.

{{< imgwithlink src="images/anka-build-cloud/instance-quotas.png" >}}


To enable this feature in your Controller configuration file, set `ANKA_ENABLE_INSTANCE_QUOTAS=true` and `ANKA_ENABLE_AUTH="true"`.

{{< hint warning >}}
**IMPORTANT UPGRADE NOTE FOR HIGH AVAILABILITY SETUPS:**
Quotas must remain disabled until every controller is upgraded. To disable, set `ANKA_ENABLE_INSTANCE_QUOTAS=false` or remove the ENV entirely.
{{< /hint >}}

To use this feature, you need to create a new [Permission Group]({{< relref "anka-build-cloud/Advanced Security Features/authorization/index.md#groups--group-permissions" >}}). Then, you can specify the max instance quota for the group under the Instance Quotas page.