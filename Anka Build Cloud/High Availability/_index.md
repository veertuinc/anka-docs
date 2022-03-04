---
date: 2019-12-12T00:00:00-00:00
title: "High Availability"
linkTitle: "High Availability"
weight: 6
description: >
  General information about setting up High Availability for the Anka Build Cloud
---

The Anka Build Cloud is composed of several components that can be split up, made redundant, and load-balanced. They each run within docker containers or on macOS as a native service.

- Our macOS native package combines all components into a single service/binary for ease of use. While you can separate them with specific configuration options and packages, we recommend considering using the docker containers instead.

- Our docker package contains a controller, registry, and etcd container inside of a single docker-compose.yml. This is by far the easiest to work with to create HA.

## Controller

The controller is stateless and saves all its data on `etcd`. Therefore it is possible to scale the controller application _horizontally_ by replicating it to multiple instances. 

{{< hint info >}}
For more information about clustering etcd please refer to https://etcd.io/docs/v3.4/op-guide/clustering/. We highly recommend understanding [the `-initital-cluster-state` option](https://etcd.io/docs/v3.4/op-guide/configuration/#--initial-cluster-state) as it typically causes headache when trying to set up a cluster: a state of `new` will not work unless the etcd storage/data directory is empty.
{{< /hint >}}

{{< hint info >}}
When running the Anka Build Cloud on a large scale, it is recommended to use a load balancer like nginx or haproxy. It’s also possible to divide traffic between load balancer instances with hardware or virtual tools (beyond the scope of this document).
{{< /hint >}}

{{< hint info >}}
When joining agents, the controller URL should be pointed to the load balancer/virtual IP.
{{< /hint >}}

Here’s a diagram of an environment running an etcd cluster and multiple controllers:

![controller-ha]({{< siteurl >}}images/anka-build-cloud/high-availability/controller-ha.png)

{{< hint info >}}
"Agent" in the above image is the anka agent running on the Anka Build nodes. This service is started after you issue an `ankacluster join`.
{{< /hint >}}

## Registry

The registry uses the local disk to save its data. We can scale the registry using one of the following approaches:

### Shared Storage

Multiple registry servers can be connected with shared storage and server behind a load balancer.

Here’s a diagram of such an environment:

![registry-ha]({{< siteurl >}}images/anka-build-cloud/high-availability/registry-shared-storage-ha.png)

### Replicated Storage

It is possible to set up multiple registry servers and replicate the registry data directory between them using a set a custom scripts/commands. If there’s a need, a write-only registry can be set up. Data from that registry will then be replicated to the other servers.

Here’s a diagram that shows that environment:

![registry-replicated-storage-ha]({{< siteurl >}}images/anka-build-cloud/high-availability/registry-replicated-storage-ha.png)
