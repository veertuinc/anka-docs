---
---

{{< hint error >}}
EOL and Deprecation list:
- Anka Build Cloud Controller and Registry versions < 1.8 EOL
{{< /hint >}}

**Once you have collected the logs, go through the below mentioned troubleshooting steps and then contact support.**

---

## Anka Build Cloud Controller / Registry

### Troubleshooting checklist

- Check `df -h` + CPU/RAM and be sure that the host resources are not exhausted.
- Look over the controller, registry, and even agent logs on the Anka Nodes and ensure that there are no configuration errors or clear indications of what is wrong.
- Ensure that all components in your Anka environment can communicate. This includes connectivity between CI/CD tooling we do not support.

### Logs

{{< hint info >}}
You might be interested in changing the verbosity of the following logs. If so, you can set the `ANKA_LOG_LEVEL` ENV to an integer greater than 0 and restart the controller. See [our Configuration Reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}}) for more details.
{{< /hint >}}

#### Linux / Docker Package

Anka Controller and Registry services can run with linux using Docker. Logs are available via the Controller dashboard, docker logs, and via a central-logs directory stored in the Registry.

{{< hint info >}}
The Anka controller is responsible for cleaning unused logs.
{{< /hint >}}

##### Anka Controller

{{< include file="_partials/anka-build-cloud/docker-controller-logs.md" >}}

##### Anka Registry

{{< include file="_partials/anka-build-cloud/docker-registry-logs.md" >}}

---

#### Mac Package

{{< hint warning >}}
All logs larger than 1024MB older than 7 days are deleted every day (hard coded; no logrotate configuration)
{{< /hint >}}

Anka logs are available via the Controller UI under Logs as well as several directories on the macOS machine hosting it.

##### Controller

{{< include file="_partials/anka-build-cloud/mac-controller-logs.md" >}}

##### Registry

{{< include file="_partials/anka-build-cloud/mac-registry-logs.md" >}}

### Anka Build Cloud Controller Agent Logs

{{< include file="_partials/troubleshooting/controller-agent.md" >}}

{{< include file="_partials/troubleshooting/centralized-logs.md" >}}

---
