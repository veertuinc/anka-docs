## Controller-less Build Cloud (Registry Only)

This setup requires:

1. At least one Anka Node (macOS host running the Anka Virtualization software).
2. A linux container running the Anka Build Cloud Registry.
3. Your CI/CD's runner/agent installed and able to execute `anka` CLI commands to prepare and use the Anka VM. For example, install the github actions runner and then use [our action]({{< relref "intel/CI Plugins and Integrations/Controller-less/github-actions.md" >}}).

![Controller-less]({{< siteurl >}}images/what-is-anka/AWS_AnkaBuildController-lessDiagram.png)