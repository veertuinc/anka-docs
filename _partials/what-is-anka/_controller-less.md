#### Controller-less (Registry Only)

This setup requires:

1. At least one Anka Node (macOS host running the Anka Virtualization software).
2. The Anka Build Cloud Registry (Linux container, [macOS binaries]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry-mac.md" >}}), or [Helm](https://github.com/veertuinc/helm-charts/tree/main/charts/anka-build-cloud)).
3. Your CI/CD's runner/agent installed and able to execute `anka` CLI commands to prepare and use the Anka VM. For example, install the github actions runner and then use [our action]({{< relref "plugins-and-integrations/controller-less/github-actions.md" >}}).

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Controller-less]({{< siteurl >}}images/what-is-anka/AWS_AnkaBuildController-lessDiagram.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}
