---
date: 2019-07-03T22:24:47-05:00
title: "Using Teamcity and the Anka Build Cloud"
linkTitle: "Teamcity"
weight: 3
description: >
  Instructions on how to use Teamcity with the Anka Build Cloud
aliases:
  - "/docs/ci-plugins-and-integrations/teamcity/"
  - "/docs/plugins-and-integrations/teamcity/"
---

Teamcity is a popular CI/CD tool that can be used to build and test software projects. The Anka Build Cloud can be used to run builds and tests in a cloud environment, and it can be integrated with Teamcity to provide a complete CI/CD solution needing ephemeral macOS VMs/containers.

The Anka Teamcity plugin allows you to communicate with your Anka Build Cloud Controller to create, manage, and delete VMs/containers on demand. You'll create a Cloud Profile in Teamcity to define your Controller, VM Template, Tag, and other options. Once defined, you can assign that Cloud Profile to an Agent Pool, or keep it in the default pool. 

As of 1.9.0, our plugin will install the Teamcity Build Agent for you into the VM Template/Tag under the path specified in your Cloud Profile.

## VM Template & Tag Requirements

1. **In the VM**, install the proper JRE/JAVA version. We recommend [Zulu](https://www.azul.com/downloads/?version=java-17-lts&package=jre#zulu). **As of plugin 1.9.0, you will need Java 17 or higher.**
1. **If using Shared Networking mode:** In the VM Template, make sure remote login is enabled (`System Preferences > Sharing`). On the host, enable SSH [port forwarding]({{< relref "anka-virtualization-cli/command-line-reference.md#modify-nameuuid-port" >}}) for your VM Template using the Anka CLI: `sudo anka modify <VM Template name> add port --guest-port 22 ssh`. _We recommend not specifying --host-port._

## Usage

1. [Download the latest plugin zip.](https://veertu.com/downloads/ankabuild-tc-latest/).
2. Upload it to your Teamcity.
3. Edit your project and under Cloud Profiles add a new Profile. For Cloud type, choose Anka Build Cloud and specify the URL for your Controller and the Server URL. The Server URL may look populated, but it will not work unless you manually type it.
4. Choose the template you created for Teamcity and specify any other required fields.
{{< hint warn >}}
Be sure to set **Agent Path** to the agent root directory, not a folder inside of it like bin, etc.
{{< /hint >}}
5. Run your job and then watch the Controller's Instances page to ensure an instance is starting.

{{< hint warn >}}
**Be sure to review our [troubleshooting guides]({{< relref "plugins-and-integrations/troubleshooting/_index.md#teamcity" >}}) for Teamcity should you have any problems.**
{{< /hint >}}

---

## Release Notes

- **1.8.0**: null-check of rootCAParam prior to empty-check https://github.com/veertuinc/teamcity-anka-cloud/pull/12 + Various security updates.
