---
---

#### Groups + Group Permissions

{{< hint warning >}}
Do not confuse Node Groups with Groups/Group Permissions.
{{< /hint >}}

Under the Permissions section of the Controller UI (`https://<controller address>/#/permission-groups`), let's create a group for our nodes to be able to connect.

1. First choose the Controller Component from the drop down.

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-choose-component.png" >}}

2. Next, create a new group named `node` by clicking the [+] button.

3. You can now target and add specific **Permissions** or **Resources** for the Group. Click the circular (💻) icon on the right to highlight what permission should be set for Nodes to communicate with the Controller. Then check/enable the highlighted permissions and click Save Permissions at the bottom of the page. *Important:* Finally, do the same but under the Registry Component.

{{< hint warning >}}
Note: We'll *not* be setting **Resources** right now.
{{< /hint >}}

{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-highlight-node-perms.png" >}}
{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-set-perms.png" >}}

4. The group can now be attached to a specific [Authentication Credential]({{< relref "anka-build-cloud/Advanced Security Features/_index.md#authentication" >}}) and the credential used to access and perform the permitted actions. For example, create a [UAK]({{< relref "anka-build-cloud/Advanced Security Features/uak-tap-authentication.md" >}}) and attach the group.

{{< hint info >}}
Be sure to download the key if you're creating a new UAK.
{{< /hint >}}

{{< rawhtml >}}<center>{{< /rawhtml >}}
{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-set-group-on-uak.png" >}}
{{< rawhtml >}}</center>{{< /rawhtml >}}

You will now see the UAK in the list.

{{< rawhtml >}}<center>{{< /rawhtml >}}
{{< imgwithlink src="images/anka-build-cloud/advanced-security-features/perm-groups-uak-list.png" >}}
{{< rawhtml >}}</center>{{< /rawhtml >}}

The node certificate now has permissions to perform the specifically set actions against all Resources (if Resource Management is disabled).

5. You can now try joining the node to the Controller using the UAK and confirm it's all joined by checking the Controller Nodes page, or the agent logs.

```bash
❯ sudo ankacluster join http://anka.controller:8090 --api-key-file ~/node.cer --api-key-id "node"
Testing connection to the controller...: Ok
Testing connection to the registry...: Ok
Success!
Anka Cloud Cluster join success
```

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
| `view_prmission_groups` | gives the user permission to view Group Permissions |
| `update_permission_groups` | gives the user permission to update Group Permissions |
| `delete_permission_groups` | gives the user permission to delete Group Permissions |

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
| `view_prmission_groups` | gives the user permission to view Group Permissions |
| `update_permission_groups` | gives the user permission to update Group Permissions |
| `delete_permission_groups` | gives the user permission to delete Group Permissions | -->