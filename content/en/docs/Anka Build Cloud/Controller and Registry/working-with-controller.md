---
date: 2019-12-12T00:00:00-00:00
title: "Working with the Controller"
linkTitle: "Working with the Controller"
weight: 5
description: >
  How to work with the Anka Controller.
---

Anka Controller is the central management system of Anka Build and provides a simple and extensible interface for provisioning and managing on-demand macOS VMs on a cluster of mac hardware (Anka Build nodes).
If you use CI tools like Jenkins, Teamcity. GitLab CI, BuidKite, integrate them with Anka controller using plugins or controller REST APIs to provision macOS VMs on-demand for CI job requests.  

You can work with controller through the web portal interface, REST APIs or the CI plugins.

## Controller Portal
Access it by going to your controller IP - `http://<controllerIP>:port`.

You can view the status of your Anka Build macOS cloud from this UI and also perform basic management operations.  
### Dashboard View
This view displays the total active build nodes, running VM instances, instance run capacity utilization, registry storage consumption, average cpu and ram utilization across the entire cloud. 

![image1](/images/using-controller/image1.png)

### Nodes View
Click on nodes to go to node list view.

You can view all active build nodes, instances running on them, their cpu and ram utilization. From this view, you can modify the concurrent VM capacity for each node.

![image2](/images/using-controller/image2.png)

### Templates View
Click on templates to look at all VMs stored in the registry.

![image3](/images/using-controller/image3.png)

Click on an individual template to view all versions/tags for that template.

![image4](/images/using-controller/image4.png)

Click on distribute to all nodes or select specific Nodes to pre-load the most frequently used Vm templates for your build/test jobs on all build nodes. This will reduce the time for first time job execution on Anka Build cloud. Controller manages disk space on the Build Nodes and deletes VM templates that are not used for CI build and test jobs.

> Note - Once a job executes on a build node in a specific VM, the original VM template used for this job is cached on the node. Hence, any subesequent job executions don't download VM from the registry. The VM template is only downloaded when there is a brand new VM template or a new tag to an existing VM template. Download of a VM template with a new tag is only incremental.

![image5](/images/using-controller/image5.png)

Distribution to nodes is complete.

![image6](/images/using-controller/image6.png)

### Instances View
Click on Instances to get a list of all running instances on the cloud.

![image7](/images/using-controller/image7.png)

### Manually starting instances
Click on create instance to manually start instances using a specific VM template/tag on the cloud.

![image8](/images/using-controller/image8.png)

### Accessing Error logs
Starting from Controller release version 1.0.12, logs will be available for download from the Controller Management portal for error scenarios during VM provisioning.

![image9](/images/using-controller/image9.png)

![image10](/images/using-controller/image10.png) 

---

## Enterprise License Features

### Node Groups

{{< include file="shared/content/en/docs/Anka Build Cloud/Controller and Registry/partials/_node_groups.md" >}}

### Priority Scheduling

{{< include file="shared/content/en/docs/Anka Build Cloud/Controller and Registry/partials/_priority-scheduling.md" >}}

### USB Device Control (Controller API)

{{< include file="shared/content/en/docs/Anka Build Cloud/Controller and Registry/partials/_usb-device-control-api.md" >}}
