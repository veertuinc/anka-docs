---
date: 2019-07-26
title: "With Kubernetes"
linkTitle: "With Kubernetes"
weight: 1
description: >
  How to setup your Anka Build Cloud for High Availability using Kubernetes
---

As the popularity and maturity of Kubernetes grows, we've also received an increase of questions from customers on using it to deploy the Build Cloud Controller & Registry. We'll describe what High Availability looks like for the Anka Build Cloud Controller & Registry software.

There are many ways to deploy your Anka Build Cloud into Kubernetes. We have a helm chart built for AWS that will get you up and running quickly. You can also use it for reference when creating your own, should you wish to have a more advanced configuration: https://github.com/veertuinc/helm-charts/tree/main/charts/anka-build-cloud

## Notes on ETCD

Running ETCD on Kubernetes can be problematic for many reasons. Here are a list of things to consider before you design your deployments:

1. ETCD is highly sensitive to disk IO. If your PV is slow, etcd will timeout queries and things will not work as expected -- especially under heavy load which you may not see until production testing, so be sure to address this sooner than later.
2. ETCD pod\[s] running on a cluster with multiple nodes must either have `nodeAffinity` or distributed data across the nodes. If the etcd pod is rescheduled on a different node in the cluster, it will lose its data, and that data is critical for the Controller to function. A sign that the pod was rescheduled: you'll wake up one day and Anka Node and permission groups, auth keys, etc will be gone.

<!-- Our [Getting Started GitHub Repository](https://github.com/veertuinc/getting-started/tree/master#anka-build-cloud--kubernetes-anka_build_cloudkubernetes) has a few scripts available to help you run the Anka Build Cloud on your macOS device using [minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/). These scripts will generate the necessary YAML you can use to deploy it into your minikube (We cannot guarantee they will work properly outside of minikube and recommend against production use). You may, of course, choose to use Deployments instead of our Statefulsets and even split your controller and registry into separate pods.

The YAML we generate includes:

- A namespace and context for Anka
- 3 ETCD pods
- 2 Build Cloud pods, each with two containers housing the Controller and Registry
- A shared volume for both Registry containers so they see the same VM Templates and Tags
- Load Balancers for each service

Here is a visual diagram for this:

{{< rawhtml >}}<center>{{< /rawhtml >}}
![installer with pkg]({{< siteurl >}}images/kubernetes-anka-build-cloud-ha-diagram.png)
{{< rawhtml >}}</center>{{< /rawhtml >}}

We must mention that the differences between how our customers have deployed Kubernetes (AWS vs. google cloud vs. self-hosted) can require significant changes from the examples we provide. We've kept our examples simple to show the configuration and the necessary connections between components. For example, exposing your load balancer for the Controller service on the network may not be required (internal DNS might be available since your CI/CD tool is also inside of Kubernetes). Or, it may require approval and a specific process due to security.

> We recommend communicating with your DevOps/Kubernetes team to handle anything missing.

Once it's up and running, you can [join your nodes to the Controller load balancer IP/hostname.]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}})

> You will also need to expose the Registry load balancer if you want to connect your laptop to push new VM Templates or Tags. -->

## Answers to Frequently Asked Questions

- From our experience, the default nginx ingress controller doesn't handle large file transfers (pushing your VM template to the registry for example). You will need to tweak the configuration of nginx to allow for large file transfer and so that the default timeouts are not met for long running transfers. You can find annotation documentation for the kubernetes ingress-nginx [here](https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md#custom-timeouts) Example:
    ```
    metadata:
      annotations:
        nginx.ingress.kubernetes.io/proxy-body-size: "0"
        nginx.ingress.kubernetes.io/proxy-max-temp-file-size: "0"
        nginx.ingress.kubernetes.io/proxy-buffering: "off"
        nginx.ingress.kubernetes.io/proxy-request-buffering: "off"
    ```
- Older versions of NLB have an immutable idle connection timeout value of 350s. This can cause registry push/pull actions to timeout. You'll need to create an ingress ALB that accepts longer connections or use the newer NLBs that allow this to be changed too.
- If the registry pod has resource limits which are hit, it can be rescheduled on another host while it's performing actions. We recommend avoiding this as much as possible.
- Per https://github.com/kubernetes/kubernetes/issues/43916, memory request limits for the registry may cause it to frequently restart due to the linux kernel defaulting to using lots of cached memory for disk IO operations. It's best to avoid placing memory limits for the registry when running in Kubernetes.
- EFS as a backend for the registry storage can be very slow and is not recommended.
- AWS Network Load Balancers (NLB) do not support mutual TLS authentication (mTLS). For mTLS support, you need to create a TCP listener instead of a TLS listener. When configured this way, the load balancer will pass through the request as-is, allowing you to implement mTLS on the target, i.e. NGINX.
