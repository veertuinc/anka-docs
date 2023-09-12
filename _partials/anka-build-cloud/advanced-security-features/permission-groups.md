---
---

#### Permission Groups


Enabling **Permission Groups** can be done in your Controller and Registry configuration:

- `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` works for both combined (macOS) and standalone (docker) packages.
- `ANKA_ENABLE_REGISTRY_AUTHORIZATION` is for the macOS combined (controller + registry in one) package only.
- `ANKA_ENABLE_AUTHORIZATION` is only for the standalone (macOS or docker) registry packages.

Once enabled, you can set up your first Group from your Controller's `https://<controller address>/#/permission-groups` page.

First choose the Component from the drop down:

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-choose-component.png" >}}

Next, create a new group by clicking the + button:

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-create-group.png" >}}

You can now target and add specific permissions for the group:

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-set-perms.png" >}}

{{< hint info >}}
Pro Tip: There are circular buttons on the right to assist in knowing which to choose based on the use-case.
{{< /hint >}}

The group can now be attached to a specific [Authentication Credential]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) and the credential used to access and perform the permitted actions.

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