---
title: "Setting up the Controller & Registry on macOS"
toc_hide: true
weight: 1
---

Welcome! This tutorial guides you through setting up your Anka Build Cloud Controller & Registry on MacOS.

## Checklist

- [ ] Set up the Anka Build Cloud components.
- [ ] Get Orientated.

## MacOS

{{< hint info >}}
The macOS package will install and run using Apple's Rosetta. There is no native arm package at this time.
{{< /hint >}}

### Necessary Hardware

1. A Mac to install the Anka Controller and/or Registry.
2. A second Mac to install the Anka CLI (the "Node").

> You can complete this tutorial with only one machine running Mac OS, but it's not recommended.

{{< include file="_partials/anka-build-cloud/_what-we-are-doing-mac.md" >}}

#### Download the Controller and Registry packages

Download links are available on the {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page." >}}

If you are more comfortable with the command line, you can download the file with curl:

##### Controller

##### ARM64 / Silicon

```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/anka-build-cloud-controller-darwin-arm64-latest) | cut -d/ -f5) https://veertu.com/downloads/anka-build-cloud-controller-darwin-arm64-latest
```

##### AMD64 / Intel

```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/anka-build-cloud-controller-darwin-amd64-latest) | cut -d/ -f5) https://veertu.com/downloads/anka-build-cloud-controller-darwin-amd64-latest
```

##### Registry

##### ARM64 / Silicon

```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/anka-build-cloud-registry-darwin-arm64-latest) | cut -d/ -f5) https://veertu.com/downloads/anka-build-cloud-registry-darwin-arm64-latest
```

##### AMD64 / Intel

```shell
curl -S -L -o ~/Downloads/$(echo $(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/anka-build-cloud-registry-darwin-amd64-latest) | cut -d/ -f5) https://veertu.com/downloads/anka-build-cloud-registry-darwin-amd64-latest
```


#### Install the packages

Double click on the .pkg to start the UI install process. Or, you can install the package using the command line:

  ```shell
  sudo installer -pkg ~/Downloads/{pkg here} -target /
  ```

#### Verify your installation

```shell
❯ sudo anka-controller status; sudo anka-registry status
Anka Controller is Running
Anka Registry is Running
```

#### Orientation

##### Controller

- Command: `anka-controller [start|stop|restart|status|logs]`
- Default Port: `80`
- Controller binary: `/Library/Application Support/Veertu/Anka/bin/anka-controller`  
- Launchd daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`
- Configuration file: `/usr/local/bin/anka-controllerd`
- Logs: `/Library/Logs/Veertu/AnkaController`
- ETCD data will be saved in: `/Library/Application Support/Veertu/Anka/anka-controller`

##### Registry

- Command: `anka-registry [start|stop|restart|status|logs]`
- Default Ports: `8089`
- Registry binary: `/Library/Application Support/Veertu/Anka/bin/anka-registry`  
- Launchd daemon: `/Library/LaunchDaemons/com.veertu.anka.registry.plist`
- Configuration file: `/usr/local/bin/anka-registryd`
- By default, Registry storage is set to: `/Library/Application Support/Veertu/Anka/registry`

{{< hint warn >}}
Changing the location from the default may require that you give Full Disk Access to the Registry binary. This is done under System Preferences > Privacy & Security.
{{< /hint >}}

#### Configuration

You can find a list of ENVs available to use and examples in your configuration in our [Config Reference]({{< relref "anka-build-cloud/configuration-reference.md" >}}).

#### Logging

You can modify the destination in the `/usr/local/bin/anka-controllerd` and `/usr/local/bin/anka-registryd` file ([reference]({{< relref "anka-build-cloud/configuration-reference.md#logging" >}})).

{{< include file="_partials/anka-build-cloud/mac-controller-logs.md" >}}

{{< include file="_partials/anka-build-cloud/mac-registry-logs.md" >}}

{{< hint info >}}
The log level can be modified from the default 0 value. The higher the number, the more verbose the logging. To set this, you'll add `-v 10` at the end of `/Library/Application\ Support/Veertu/Anka/bin/anka-controller` inside of your `/usr/local/bin/anka-controllerd` (or anka-registryd).
{{< /hint >}}

{{< hint info >}}
You can also watch the logs live (similar to tail -f)

```shell
sudo anka-controller logs
```
{{< /hint >}}
 
### Link the Anka Virtualization Node to the Controller

Great! Now that we have our Anka Controller & Registry up and running, let's add Nodes!

> Perform the following steps on the Node where you created your first VM Template.

#### Add the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

#### Push the VM to the Registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_push-vm-to-registry.md" >}}

---

## What next?

- [Prepare and join your machines to the Build Cloud]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).  
- Connect the cloud to your [CI software]({{< relref "plugins-and-integrations/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "anka-build-cloud/working-with-controller-and-api.md#rest-api">}}).  
