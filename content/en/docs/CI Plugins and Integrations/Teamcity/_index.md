---
date: 2019-07-03T22:24:47-05:00
title: "Using TeamCity and Anka Build Cloud"
linkTitle: "TeamCity"
weight: 5
description: >
  Instructions on how to use TeamCity with Anka Build Cloud
---

The TeamCity **Anka Build Cloud** provides a quick way to integrate Anka Build Cloud with TeamCity. The plugin will dynamically provision Anka VM instances for building, testing, and more.

## VM Template & Tag Requirements

The TeamCity plugin requires a VM with Java, SSH Remote Login, port-forwarding, and TeamCity:

1. In the VM, install the proper OpenJDK version.
    - Within Jenkins, visit **/systemInfo** (`System Properties`) and look for `java.version`.
    - Use the value to determine the proper OpenJDK version you need to download and install in your VM Template. For example if the `java.version` is set to `1.8.0_242`, you can download and install the [AdoptOpenJDK jdk8u242-b08.pkg](https://github.com/AdoptOpenJDK/openjdk8-binaries/releases).
2. In the VM, make sure remote login is enabled (`System Preferences > Sharing`).
3. On the host, enable [port forwarding]({{< relref "docs/Anka Virtualization/command-reference.md#example---add-port-forwarding" >}}) for your VM Template using the Anka CLI. _We recommend not specifying --host-port._
4. Download TeamCity into the VM under your user's home directory:
    ```shell
    cd /Users/anka
    curl -O -L https://download.jetbrains.com/teamcity/TeamCity-$TEAMCITY_VERSION.tar.gz
    tar -xzvf TeamCity-$TEAMCITY_VERSION.tar.gz
    mv Teamcity/BuildAgent /Users/anka/buildAgent
    ```
5. Remove everything from the buildAgent properties file: `echo >> buildAgent/conf/buildagent.properties` (Do not delete it)
6. Start the TeamCity agent inside the VM with `sh BuildAgent/bin/mac.launchd.sh load` and then wait a minute before continuing.
7. `sudo anka suspend <VM Template name>`
8. `sudo anka registry push <VM Template name> <Tag name>`

## Install and Configure the Anka Plugin in TeamCity

> These steps are based on TeamCity 2019.2.4. Your version and UI may differ slightly.

1. Install the **Anka Build Cloud** plugin: Browse to `Administration > Plugins List` and upload the `anka-build-tc-XX.zip`.

2. Under **Edit Project** for your project, navigate to **Cloud Profiles**.

3. Click on **Create new profile** and choose `Anka Build Cloud` for the **Cloud type**.

4. It's a good idea to set the **Server URL** to your TeamCity address and port (including `http[s]://`). Otherwise, TeamCity could pass the wrong serverUrl to the VM (if you're running TeamCity on a non-default port).

4. For **Additional terminate conditions**, check the box for **After first build**.

5. For **Controller URL**, enter Anka Build Cloud Controller IP and port. Default port is 80.

6. Once the connection is successful to the Controller, **Image Name** will show all VM templates from the Registry. Select the one you want to use for your project.
  
7. **Image Tag** will show all Tags for the selected VM Template (`"Image Name"`). Select the one you want to use or leave it to `Latest` to always use the latest Tag.

8. For **Group** you can define the specific group to run the VMs on (Available only in Enterprise and Enterprise Plus Tiers).

9. **SSH User** and **SSH Password** are the default, unless you've changed the user for the VM, logins (anka/admin).

10. **Agent Path Select** is the path to TeamCity agent location in the VM (don't modify this unless you installed TeamCity outside of the anka user home directory.

12. Enter **Priority** the VM will take when scheduling the instance start (Available only in Enterprise and Enterprise Plus tiers).

Once saved, you will now see the object under your Agents page named after the `Image Name` you chose in the configuration/setup.

Anytime you execute a build for the project, it will request a new VM from the Anka Build Cloud Controller and wait for it to start. Once started, the plugin will SSH into the VM and add the necessary values to the `buildAgent.properties` config. The Agent will then start and run your job. It's important to ensure that access from TeamCity to the Node (which hosts the VMs), VM (using port-forwarding), and then from the VM itself back to TeamCity is possible.


## Common Issues

1. [Build stuck in queue and no Agents spawning]({{< ref "docs/Anka Build Cloud/Troubleshooting Guides and Logs/guides/teamcity-plugin/build-stuck-in-queued.md">}})
