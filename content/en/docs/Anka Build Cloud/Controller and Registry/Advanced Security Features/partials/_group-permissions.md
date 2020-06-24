## Managing Permissions Groups

> **This feature requires Enterprise Plus**

Permission groups are configurable from your Controller's `https://<controller address>/admin/ui` page.

> The permission groups here differ from the groups you assign to nodes

When creating your certificates, you'll want to specify CSR values using openssl's `-subj` option. For example, if we're going to generate a certificate so our Jenkins instance can access the Controller & Registry, you'll want to use something like this:

```shell
-subj "/C=$COUNTRY/ST=$STATE/L=$LOCATION/O=MyOrgName/OU=$ORG_UNIT/CN=Jenkins"
```

> The two required values to set are `O=` and `CN=`

> Spaces are not currently supported in `O=`

Within the Controller, we use **`O=`** as the **permission group** and **`CN=`** as the **username**.

Once you've setup your certificate, you can then create a _New Group_ in the /admin/ui panel. The _Group Name_ will be **MyOrgName**, like we used in the `-subj` above.

The **Available Permissions** will display all of the permissions we can assign to the group. For Jenkins, we'll want the following:

- `get_groups`
- `get_registry_disk_info`
- `head_push_vms`
- `list_images`
- `list_nodes`
- `list_vms`
- `pull_vm`
- `push_vm`
- `registry_list`
- `save_image`
- `start_vm`
- `terminate_vm`
- `update_vm`
- `upload_file`
- `view_logs`

These permissions will allow the Anka Jenkins Plugin to communicate with the Controller & Registry.