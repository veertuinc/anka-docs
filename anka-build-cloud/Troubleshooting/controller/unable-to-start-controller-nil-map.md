---
title: "panic caught: assignment to entry in nil map"
weight: 1
---

## Scenario

Starting the Controller fails with:

```
. . .
I0421 10:56:45.271671   23232 migrations.go:35] Performing DB migrations
I0421 10:56:45.272956   23232 config.go:80] Importing persisted config
I0421 10:56:45.273341   23232 panic.go:884] Exiting import persisted config
E0421 10:56:45.273366   23232 panic_logger.go:15] panic caught: assignment to entry in nil map
E0421 10:56:45.273910   23232 panic_logger.go:16] goroutine 1 [running]:
runtime/debug.Stack()
	/usr/local/go/src/runtime/debug/stack.go:24 +0x65
veertu.com/veertu/ankaCloud/utils.OutputPanicAndExit()
. . .
```

## Solution

{{< hint warning >}}
You will have to re-create groups and attach them to the nodes in your Controller as this deletes the etcd data.
{{< /hint >}}

1. Stop the Controller
2. `sudo rm -rf /Library/Application\ Support/Veertu/Anka/anka-controller/member`
3. Start the Controller


## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
