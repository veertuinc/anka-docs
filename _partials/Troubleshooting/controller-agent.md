---
---

- Logs location: `/var/log/veertu`
- Format: `regd.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`

There are 5 type of symlinks in the logs location pointing to the latest active logs. The verbosity of the logs are from highest (INFO) to the lowest (ERROR):

- `anka_agent.INFO` - contains all of the below except for CMD log.
- `anka_agent.WARNING` - contains WARNNIGS & ERRORS.
- `anka_agent.ERROR` - contains just ERRORS.
- `anka_agent.FATAL` - only FATAL ERRORS.
- `anka_agent.CMD` - (new in 1.20.0) contains the various anka commands the agent is executing on the host as well as the returned data.

You can also read and download the logs via the UI in the Controller dashboard. Though, only relevant if you've [joined your Node to the Build Cloud Controller & Registry]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).
