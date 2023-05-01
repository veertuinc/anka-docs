---
---
Using the `docker logs` command: `docker logs --follow <ControllerContainerName>`

{{< hint info >}}
The controller is an API, so all API connections made to it from Anka-agent or CI platforms(Jenkins) logs here. If a vm fails to start it suggests first to check this logs.
{{< /hint >}}
