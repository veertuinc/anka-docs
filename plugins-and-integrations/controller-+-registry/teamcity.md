---
date: 2019-07-03T22:24:47-05:00
title: "Using Teamcity and the Anka Build Cloud"
linkTitle: "Teamcity"
weight: 3
description: >
  Instructions on how to use Teamcity with the Anka Build Cloud
---

## VM Template & Tag Requirements

1. **In the VM**, install the proper JRE/JAVA version. We recommend [Zulu](https://www.azul.com/downloads/?version=java-11-lts&package=jre#zulu).
1. Install the Build Agent into the VM Template/Tag under ~/. [Example of how we install it.](https://github.com/veertuinc/getting-started/blob/1ef4ed31eead3dccd900e16912d487b1befcb5a5/create-vm-template-tags.bash#L161)
1. **If using Shared Networking mode:** In the VM Template, make sure remote login is enabled (`System Preferences > Sharing`). On the host, enable SSH [port forwarding]({{< relref "anka-virtualization-cli/command-line-reference.md#modify-nameuuid-port" >}}) for your VM Template using the Anka CLI: `sudo anka modify <VM Template name> add port --guest-port 22 ssh`. _We recommend not specifying --host-port._

## Usage

1. [Download the latest plugin zip.](https://veertu.com/downloads/ankabuild-tc-latest/).
2. Upload it to your Teamcity.
3. Edit your project and under Cloud Profiles add a new Profile. For Cloud type, choose Anka Build Cloud and specify the URL for your Controller and the Server URL (if it differs from the default).
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
