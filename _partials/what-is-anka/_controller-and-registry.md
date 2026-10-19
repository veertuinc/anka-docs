#### Controller + Registry

This setup requires:

1. At least one Anka Node (macOS host running the Anka Virtualization software).
2. The Anka Build Cloud Controller & Registry (Linux container, [macOS binaries]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry-mac.md" >}}), or [Helm](https://github.com/veertuinc/helm-charts/tree/main/charts/anka-build-cloud)). The Controller pulls templates only from the Anka Registry. To use OCI tags with the Controller, build the template from them and push it to the Anka Registry. See [Combining both registries]({{< relref "templates-and-registries/combining-both-registries.md" >}}).
3. Our plugin installed in your CI/CD tool (like the [Anka Jenkins Plugin]({{< relref "plugins-and-integrations/controller-+-registry/jenkins.md" >}})). See a full list of plugins available on our [CI Plugins and Integrations]({{< relref "plugins-and-integrations/_index.md" >}}) page.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Controller+Registry]({{< siteurl >}}images/what-is-anka/AWS_AnkaBuildDiagram.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}
