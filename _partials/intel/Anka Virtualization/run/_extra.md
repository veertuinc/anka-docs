---
---

{{< hint info >}}
Anka `run` will source user "rc" files using `{default shell} -l`.
{{< /hint >}}

{{< hint warning >}}
Anka `run` will wait for all the stdout/err and ports to be closed before actually ending the command. This can make things seem like they are stuck while executing scripts that background processes.
{{< /hint >}}
