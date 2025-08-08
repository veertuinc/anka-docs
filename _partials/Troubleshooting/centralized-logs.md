

### Centralized Logs

If `ANKA_ENABLE_CENTRAL_LOGGING` is set to true in your controller and registry config, logs for all Build Cloud services will be stored in the registry's data directory under `{registryStorageLocation}/files/central-logs/`.

You will find historical and current anka_agent logs from each of your nodes, the controller logs, and if applicable the registry logs.

- On macOS: The default is `/Library/Application\ Support/Veertu/Anka/registry/files/central-logs/`

- On docker: You set the storage location in your docker-compose.yml, but it will still be under `{ANKA_BASE_DIR}/files/central-logs/` (default: `/mnt/vol/files/central-logs/`)

{{< hint info >}}
When a VM on a node fails, the agent and VM logs will be stored under /mnt/vol/files/logs/ under a folder inside labeled with the UUID of the VM that failed.
{{< /hint >}}

{{< hint warning >}}
**Important:** Logs stored in the registry are NOT automatically cleaned up. You will need to perform housekeeping to remove old logs every once in a while depending on your storage size.
{{< /hint >}}