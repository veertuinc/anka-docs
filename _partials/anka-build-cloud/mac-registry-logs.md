---
---

- Logs location: `/var/log/veertu`

- Format: `regd.HOSTNAME.USER.LOG.LOGTYPE.TIMESTAMP`

There are 4 type of symlinks in the logs location pointing to the latest active logs. The verbosity of the logs are from highest (INFO) to the lowest (ERROR):

- `regd.INFO` - contains ALL logs.
- `regd.WARNING` - contains WARNINGS & ERRORS.
- `regd.ERROR` - contains just ERRORS.
- `regd.FATAL` - only FATAL ERRORS.

You can also read and download the logs via the UI in the Controller dashboard. Only relevant if you've [joined your Node to the Build Cloud Controller & Registry]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).