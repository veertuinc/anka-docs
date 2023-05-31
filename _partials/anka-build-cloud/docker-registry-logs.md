---
---

- Logs location: Registry storage directory under `files/central-logs`.

1. Using the `docker logs` command: `docker logs --follow <RegistryContainerName>`.

2. The Registry and Controller logs share the same file and are available under the Controller's `Logs` > `Service Name: Controller`.

    ![agent logs]({{< siteurl >}}images/anka-build/logs/dashboardlogs.png)