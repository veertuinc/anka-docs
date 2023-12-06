---
---

```shell
/Library/Logs/Veertu/AnkaController/anka-controller.INFO
/Library/Logs/Veertu/AnkaController/anka-controller.WARNING
/Library/Logs/Veertu/AnkaController/anka-controller.ERROR
```

1. Show logs by command: `sudo anka-controller logs` (similar to `tail -f`)

2. There are 4 types of log files, in the snapshot you can see log files **without** ID, they are **LINK** files- point to the latest log been created ( the last active vm) , each vm can generate all of the log types below. the robosety of the logs are from highest(INFO) to the lowest(ERROR), you can check this files using 'tail' command:

- `anka-controller.INFO` - contains ALL logs.
- `anka-controller.WARNING` - contains WARNINGS & ERRORS.
- `anka-controller.ERROR` - contains just ERRORS.
- `anka_agent.FATAL` - Only FATAL ERRORS (both controller and agent).

{{< hint info >}}
The mac controller package relies on an internal ETCD database. Logs for ETCD will be included in the controller logs, but by default they are set to be non-verbose.
{{< /hint >}}
