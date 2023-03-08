---
---

By default, `ANKA_ENABLE_AUTH` will not use authorization/permissions and allow any certs or users to connect to all API endpoints and pages in the UI. In order to enable Authorization, you will need to include specific ENVs in your config:

- `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` works for both combined and standalone (docker) packages.
- `ANKA_ENABLE_REGISTRY_AUTHORIZATION` is for the combined (controller + registry in one binary) package only.
- `ANKA_ENABLE_AUTHORIZATION` is only for the standalone registry package.

Permission groups are configurable from your Controller's `https://<controller address>/#/permission-groups` page. You can target and add permissions for either the group name or the username (which is different between the various Advanced Security Features we offer).

{{< hint warning >}}
**This feature requires Enterprise Plus.** The regular enterprise license automatically adds all permissions to each certificate or token that is used and gives no control over them.
{{< /hint >}}

{{< hint warning >}}
This also requires that you've enabled [Root Token Authentication]({{< relref "anka-build-cloud/Advanced Security Features/token-authentication.md" >}}), giving you super user access to the controller UI and permissions.
{{< /hint >}}

{{< hint info >}}
The permission groups here differ from the groups you assign to nodes within the Controller UI.
{{< /hint >}}

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/new-permissions-management.png" >}}

<!-- 
### Controller Permissions

| Permission | Description |
| --- | --- |
| **Instances** |  |
| `list_vms` | gives the user permission to list vms |
| `start_vm` | gives the user permission to start vm |
| `terminate_vm` | gives the user permission to terminate vm |
| **Registry** | |
| `get_registry_files` | gives the user permission to get registry files (logs) |
| `view_logs` | gives the user permission to view log files in dashboard |
| `get_registry_disk_info` | gives the user permission to get registry disk info |
| `registry_list` | gives the user permission to list vms on registry |
| `registry_delete` | gives the user permission to registry delete |
| **Nodes** | |
| `list_nodes` | gives the user permission to list nodes |
| `delete_node` | gives the user permission to delete node |
| `change_node_config` | gives the user permission to change node configuration |
| **Node Groups** | |
| `create_group` | gives the user permission to create node groups |
| `get_groups` | gives the user permission to view node groups |
| `delete_group` | gives the user permission to delete node groups |
| `update_group` | gives the user permission to update node groups |
| `add_node_to_group` | gives the user permission to add a node to a node group |
| `remove_group_from_node` | gives the user permission to remove a node from node group |
| **Distribute VMs** | |
| `registry_distribute` | gives the user permission to distribute vms from registry |
| `registry_distribute_status` | gives the user permission to view distribution statuses |
| **Config** | |
| `change_config` | gives the user permission to change global configuration |
| `get_config` | gives the user permission to view global configuration |
| **Permissions and groups** | |
| `view_permissions` | gives the user permission to view the list of available permissions |
| `view_prmission_groups` | gives the user permission to view permission groups |
| `update_permission_groups` | gives the user permission to update permission groups |
| `delete_permission_groups` | gives the user permission to delete permission groups |

### Registry Permissions

| Permission | Description |
| --- | --- |
| **Information about Registry** | |
| `index` | gives the user permission to view the registry index (welcome html file) |
| `get_disk_info` | gives the user permission to get disk info |
| **List VMs** | |
| `list_vms` | gives the user permission to list vms |
| **Push VMs** | |
| `head_push_vm` | gives the user permission to “negotiate” a push (understand which files exists on the server and which files need to be sent) |
| `push_vm` | gives the user permission to push vm and create new vms or tags |
| **Pull VMs** | |
| `pull_vm` | gives the user permission to get a pull vm request (list of files needed for download and their paths) |
| `download_vm` | gives the user permission to download vm files (as given by pull_vm) |
| **Delete VMs** | |
| `delete_vm` | gives the user permission to delete a vm |
| `revert` | gives the user permission to revert vm versions |
| **File Server** | |
| `upload_file` | gives the user permission to upload a file |
| `download_file` | gives the user permission to download a file |
| **Log Server** | |
| `get_streamer` | gives the user permission to get an html streamer page (for logs) |
| `stream_log` | gives the user permission to stream a log file (as given by get_streamer) |
| `get_log_archive` | gives the user permission to download a log archive (tar.gz) |
| `send_log_event` | gives the user permission to send log events (only applies specifically to eventLog) |
| `send_log` | gives the user permission to send a log file row |
| **Permissions and groups** | |
| `view_permissions` | gives the user permission to view the list of available permissions |
| `view_prmission_groups` | gives the user permission to view permission groups |
| `update_permission_groups` | gives the user permission to update permission groups |
| `delete_permission_groups` | gives the user permission to delete permission groups | -->