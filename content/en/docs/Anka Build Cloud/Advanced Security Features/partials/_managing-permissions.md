> **This feature requires Enterprise Plus**

Permission groups are configurable from your Controller's `https://<controller address>/admin/ui` page.

> The permission groups here differ from the groups you assign to nodes within the Controller UI

The **Available Permissions** list will display all of the permissions we can assign to the group (see below for the full list). These permissions will allow plugins/users (like Jenkins) to communicate with the Controller & Registry:

#### Controller

- `get_groups`
- `get_registry_disk_info`
- `list_images`
- `list_nodes`
- `list_vms`
- `save_image`
- `start_vm`
- `terminate_vm`
- `update_vm`
- `view_logs`



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
| **Mac Address** | |
| `mac_address` | gives the user permissions to control the mac address features |
| **Token Auth** |
| `create_api_key` | |
| `delete_api_key` | |
| `get_api_key` | |
| `update_api_key` | |


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
| `delete_permission_groups` | gives the user permission to delete permission groups |
| **Token Auth** |
| `create_api_key` | |
| `delete_api_key` | |
| `get_api_key` | |
| `update_api_key` | |
