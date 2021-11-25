---
---

{{< hint info >}}
Anka `run` will source user "rc" files in a specific order:

The source sequence is:

1. /etc/profile
2. .bash_profile
3. .bash_login
4. .profile
{{< /hint >}}

{{< hint warning >}}
Anka `run` will wait for all the stdout/err and ports to be closed before actually ending the command. This can make things seem like they are stuck while executing scripts that background processes.
{{< /hint >}}
