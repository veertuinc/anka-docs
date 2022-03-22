---
---

### Centralized Logs

If `ANKA_ENABLE_CENTRAL_LOGGING` is set to true in your controller and registry config, logs for all Build Cloud services will be stored in the registry's data directory under `{registryStorageLocation}/files/central-logs/`.

You will find historical and current anka_agent logs from each of your nodes, the controller logs, and if applicable the registry logs.

- On macOS: The default is `/Library/Application\ Support/Veertu/Anka/registry/files/central-logs/`

- On docker: You set the storage location in your docker-compose.yml, but it will still be under `.../files/central-logs/`
