---
title: "Monitoring"
linkTitle: "Monitoring"
weight: 9
description: >
  The various ways in which you can monitor your Anka Build Cloud
---

In order to monitor your Anka Build Cloud, you'll need a few things: 

1. A way to collect metrics (time series database, etc)

2. A way to graph and setup alerts

## Prometheus + Grafana

[Prometheus](https://github.com/prometheus/prometheus#prometheus) is a powerful monitoring and alerting toolkit. You can use it to store Anka Controller, Registry, VM metrics to build out or integrating into existing graphing tools like [Grafana](https://github.com/grafana/grafana#get-started).

While exactly how you should install Prometheus and Grafana are outside of our bounds of support, we do have a way to set it up locally (using docker), connect it to your Build Cloud Controller, and start playing around. The script you can run to achieve this can be found on our [Getting Started Github Repo](https://github.com/veertuinc/getting-started#prometheus-exporter-prometheus).

Once the getting-started repo is cloned into my local machine, I need to start a docker container with Prometheus. I'll execute the run in docker script: `cd getting-started && ./PROMETHEUS/run-prometheus-on-docker.bash`. Once running, I'm going to visit the url and port it created for me (shown in the output of the script).

{{< rawhtml >}}<center>{{< /rawhtml >}}
![prometheus-browser](/images/anka-build-cloud/monitoring/prometheus-in-browser.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}

Prometheus on its own does not collect metrics from the Anka Build Cloud. We'll need to run the [Anka Prometheus Exporter](https://github.com/veertuinc/anka-prometheus-exporter) for that. I'll use the [Getting Started Github Repo](https://github.com/veertuinc/getting-started#prometheus-exporter-prometheus) script for this: 

```bash
❯ ./PROMETHEUS/install-and-run-anka-prometheus-on-mac.bash
]] Removing previous files and processes
]] Downloading anka-prometheus-exporter from github
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   193  100   193    0     0   1331      0 --:--:-- --:--:-- --:--:--  1331
100   656  100   656    0     0   2429      0 --:--:-- --:--:-- --:--:--  2429
100 6614k  100 6614k    0     0  7440k      0 --:--:-- --:--:-- --:--:-- 7440k
Archive:  anka-prometheus-exporter_v2.1.0_darwin_amd64.zip
  inflating: anka-prometheus-exporter
]] Running /tmp/anka-prometheus-exporter --controller-address http://anka.controller:8090 and backgrounding the process
================================================================
PID: 2323
Endpoint URL: anka.prometheus:2112
❯ {"level":"info","msg":"Starting Prometheus Exporter for Anka (2.1.0)","time":"2021-03-22T15:19:17-04:00"}
{"level":"info","msg":"Serving metrics at /metrics and :2112","time":"2021-03-22T15:19:17-04:00"}
```

This will run the exporter in the background **(it will not be available after you reboot your machine and you'll have to re-run the script)**. The exporter is setup, by default, to connect to `http://anka.controller:8090` and your controller may not be running at this URL and port. Feel free to modify the script.

Next, we'll want to confirm that the metrics are now available in Prometheus.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![prometheus-metrics](/images/anka-build-cloud/monitoring/prometheus-metrics.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}

Once we've confirmed the metric are available, you can now use the built in graph and alerting to setup alerts around `anka_node_states` to see when Node connectivity to the controller fails, `anka_instance_state_count` to see when instances are throwing errors, `anka_node_cpu_util` to trigger alarms when VMs are crippling the Node, and much more. A full list of metrics is available at https://github.com/veertuinc/anka-prometheus-exporter#exposed-metrics.

We won't go into depth with setting up Grafana as it's already covered on https://grafana.com/docs/grafana/latest/getting-started/getting-started-prometheus/.

> Grafana will need to connect to the host and port for Prometheus. This is available at `http://host.docker.internal:8095` if you used the scripts in the Getting Started repo.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![prometheus-grafana](/images/anka-build-cloud/monitoring/prometheus-grafana.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}
