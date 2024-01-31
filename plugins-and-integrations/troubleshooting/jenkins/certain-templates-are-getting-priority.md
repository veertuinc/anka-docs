---
title: "Jenkins jobs targeting certain templates are getting priority, even though my priority configs are not different"
linkTitle: "Jenkins jobs targeting certain templates are getting priority, even though my priority configs are not different"
weight: 3
---


## Scenario

You configured Anka Build Cloud through our Jenkins plugin. You also have several labels setup using different templates. The templates are `xcode14.1`, `xcode14.2`, and `xcode14.3`.

When you run the jobs, your capacity in the Controller is quickly used up, but for some reason the `xcode14.2` and `xcode14.3` are not scheduling as much and Nodes seem to be running only `xcode14.1` VMs.

## Common Causes

The main cause of this is the way Jenkins handles queuing pending jobs as well as the default settings in our Jenkins plugin. In our plugin the default configuration for **Cloud Instance Capacity** is to limit the requests Jenkins makes to the Controller to the available Capacity. Once the Anka Build Cloud Capacity is reached, Jenkins will queue up the jobs locally and not submit them to the Controller. This is the most efficient method as it doesn't overload the Controller but also doesn't create a Jenkins Node which is a lot of resources to manage for Jenkins.

However, the downside of this is that once Capacity is available, Jenkins will iterate over existing labels ORDERED BY NAME to know which to submit to the Controller. This means `xcode14.1` then `xcode14.2` will always take priority.

## Solution

The solution is to change our Plugin's **Cloud Instance Capacity** to `-1`, meaning that it will send all requests immediately to the Controller to manage priority.

{{< imgwithlink src="images/ci-plugins-and-integrations/jenkins/jenkins-cloud-instance-capacity.png" >}}

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

