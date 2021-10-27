## Using Anka

There are many ways in which our customers utilize the Anka Virtualization software and the Anka Build Cloud to achieve on-demand and single-use macOS VMs for iOS and native app building and testing. Anka enables a docker-like experience for teams to create and store project specific VM templates and tags; including start, stop, clone, suspend, modify cpu and ram for, and execution of commands inside of the VMs. 

Below are two of the most popular examples of how our customers set up Anka.
### Registry Only

This setup requires:

1. At least one Anka Node (macOS host running the Anka Virtualization software).
2. A linux container running the Anka Build Cloud Registry.
3. Your CI/CD's runner/agent installed and able to execute `anka` CLI commands to prepare and use the Anka VM. For example, install the github actions runner and then use [our action]({{< relref "intel/CI Plugins and Integrations/GitHub Actions/_index.md" >}}).

![Controller-less]({{< siteurl >}}images/what-is-anka/AWS_AnkaBuildController-lessDiagram.png)

### Controller + Registry (Build Cloud)

This setup requires:

1. At least one Anka Node (macOS host running the Anka Virtualization software).
2. A linux container running the Anka Build Cloud Controller & Registry.
3. Our plugin installed in your CI/CD tool (like the [Anka Jenkins Plugin]({{< relref "intel/CI Plugins and Integrations/Jenkins/_index.md" >}})). See a full list of plugins available on our [CI Plugins and Integrations]({{< relref "intel/CI Plugins and Integrations/_index.md" >}}) page.

![Controller+Registry]({{< siteurl >}}images/what-is-anka/AWS_AnkaBuildDiagram.png)
