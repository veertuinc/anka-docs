---
---

{{< hint warning >}}
This is an advanced feature, it allows you to have a second http interface that will be used only by the cluster’s Nodes
{{< /hint >}}

{{< hint warning >}}
You must join your nodes with `--skip-tests`.
{{< /hint >}}

{{< hint warning >}}
Auto upgrading of the Agent running on your nodes/hosts will fail since the Agent is not downloadable through the queue interface. You must manually download the proper agent pkg from https://downloads.veertu.com/#anka/ and install it on your node/host.
{{< /hint >}}
