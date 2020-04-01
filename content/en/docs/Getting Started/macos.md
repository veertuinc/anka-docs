
---
title: "Getting started with Anka Build Cloud On Mac OS"
linkTitle: "Run Anka Build Cloud on Mac OS"
weight: 2
description: >
  Set up your Anka Build Cloud on Mac OS.
---


Welcome! This tutorial will guide you through setting up your Anka Build Cloud.

## Necessary hardware

1. A Mac to install the Anka Controller & Registry.
2. A second Mac to install the Anka CLI (the "Node").

_You can complete this tutorial with only one machine running Mac OS, but it's not recommended._

## What we will do in this tutorial
1. Install Anka CLI + Create your first VM Template
2. Install Anka Controller & Registry
3. Link the Anka CLI Node to the Controller
4. Start a VM instance using the Anka Build Cloud web interface

## Step 1. Install Anka CLI + Create your first VM Template

**Perform the following steps on the machine that is intended to be the Node.**

### Install the Anka CLI

- "Nodes" are the host machines you want to run the Anka VMs. You can use any Apple hardware for this.

#### Download the latest Anka PKG
```shell
curl -L -o Anka.pkg https://veertu.com/downloads/ankabuild-latest
```
- You can find the various Anka Build packages on the [Anka Build download page](https://veertu.com/download-anka-build/).

#### Install the Anka PKG
```shell
sudo installer -pkg Anka.pkg -tgt /
```

#### Verify the installation
```shell 
anka version
```
- Output should be similar to `Anka Build Basic version 2.1.2 (build 112)`.

#### Activate your Anka license
```
sudo anka license activate <key>
```

For Anka CLI commands and options, see the [Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).

### Create a VM Template

#### Obtain the Mac OS installer

There are multiple ways to obtain the installer .app file for Mac OSX that we'll detail for you below:

1. If you have a pending upgrade to the next minor or patch version of Mac OS:
    - Within `Preferences -> Software Update -> Advanced`, make sure `Download new updates when available` is checked, but `Install macOS updates` is not. While you're still within `Software Update`, click `Update Now` but do not install the next version (Restart) until after you've created the anka VM or the Install .app under /Applications will be deleted.
    - Or on Catalina from the terminal, run `sudo softwareupdate --fetch-full-installer --full-installer-version 10.15.4` to download the installer.
    - You can also use the App Store to download the installer.
2. On any Mac OS version you can use the [installinstallmacos.py](https://github.com/munki/macadmin-scripts) script (requires python):

    - Download and run the script:  
      ```shell
      curl --fail --silent -L -O https://raw.githubusercontent.com/munki/macadmin-scripts/master/installinstallmacos.py
      sudo chmod +x installinstallmacos.py
      sudo ./installinstallmacos.py --raw
      ```

    - The script downloads an image to the location of the .py script, so further steps are necessary to create an Anka VM:
      ```shell
      mkdir -p /tmp/app
      hdiutil attach "<installinstallmacos.py image path>" -mountpoint /tmp/app
      cp -r "/tmp/app/Applications/Install Mac OS Mojave.app" /Applications/
      hdiutil detach /tmp/app -force
      rm -f "<installinstallmacos.py image path>"
      ```
3. Have your local IT department provide a network volume or download links.

#### Run Anka Create to generate the Template
Assuming you chose to download Mojave:
```shell
anka create --app /Applications/Install\ Mac OS\ Mojave.app/ 10.14.6
```

The VM creation should take around 30 minutes. You can continue on to Step 2 while you wait for this to finish.

## Step 2. Install Anka Controller & Registry

**Perform the following steps on the machine intended to run the Controller & Registry.**

### Download the Controller & Registry PKG

Download the file called "Cloud Controller & Registry (Run on Mac)" from {{< ext-link href="https://veertu.com/download-anka-build" text="Anka Build Download page" >}}.
If you are more comfortable with the command line, you can download the file with curl:
```shell
curl -S -L -o ~/Downloads/AnkaControllerRegistry.pkg https://veertu.com/downloads/ankacontroller-registry-mac-latest
```

### Install the Controller & Registry PKG

Double click on the .pkg to start the UI install process.
- Or, you can install the package using the command line:
    ```shell
    sudo installer -pkg ~/Downloads/AnkaControllerRegistry.pkg -target /
    ```

### Verify your installation
- Use the CLI status command:
  ```shell
  sudo anka-controller status
  ```
- Use curl:
  ```shell
  curl http://<ip>:8080/api/v1/status
  ```

{{< include file="shared/content/en/docs/Getting Started/partials/_controller-listening-on-80-and-orientation.md" >}}

#### [Configuration and scripts](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference)

The Anka Controller **AND Registry** command is installed into `/usr/local/bin/anka-controller`. To see what functions it has, execute the script with root privileges.
```shell 
sudo anka-controller
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```
When `sudo anka-controller start` is executed, the script will use `launchd` to load the daemon: `/Library/LaunchDaemons/com.veertu.anka.controller.plist`.
 - The Anka Controller & Registry run script is `/usr/local/bin/anka-controllerd`. This file acts as a run script **and configuration file**. You can modify it to change the default ports used by adding the proper option or ENV. For example, if you want to run the registry on a different port and use localhost, you would add the following above the $CONTROLLER_BIN line ([reference](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference)): 
    ```shell
    export ANKA_ANKA_REGISTRY="http://127.0.0.1:8081"
    export ANKA_REGISTRY_LISTEN_ADDRESS=":8081" 
    ```

#### [Logging](https://ankadocs.veertu.com/docs/anka-build-cloud/configuration-reference/#logging)

Logs are written to `/Library/Logs/Veertu/AnkaController` by default:
```shell
/Library/Logs/Veertu/AnkaController/anka-controller.INFO
/Library/Logs/Veertu/AnkaController/anka-controller.WARNING
/Library/Logs/Veertu/AnkaController/anka-controller.ERROR
```
  - You can modify the destination in the `/usr/local/bin/anka-controllerd` file.

You can also watch the logs live:
```shell
sudo anka-controller logs
```

The log level can be modified from the default 0 value. The higher the number, the more verbose the logging.

Great! now that we have our Anka Controller & Registry up and running let's add Nodes!

## Step 3. Link the Anka CLI Node to the Controller

**Perform the following steps on the Node where you created your first VM Template.**

{{< include file="shared/content/en/docs/Getting Started/partials/_push-to-registry.md" >}}

After the push is finished you should see your new Template in the "templates" section of the controller UI.

![Your first template](/images/getting-started/push-template.png)

{{< include file="shared/content/en/docs/Getting Started/partials/_join-node-to-controller.md" >}}

**Repeat this process on other Nodes that you want to join (Anka CLI needs to be installed).**

## Step 4. Start a VM instance using the Controller UI

{{< include file="shared/content/en/docs/Getting Started/partials/_start-vm-from-controller.md" >}}

You can now confirm the Instance is running from inside the Node:

- JSON output is available for scripting/automation using `anka --machine-readable`
    
```
sudo anka --machine-readable list | jq
{
  "status": "OK",
  "body": [
    {
      "status": "suspended",
      "name": "10.14.6",
      "stop_date": "2020-04-01T21:30:59.798697Z",
      "creation_date": "2020-04-01T00:00:13.656296Z",
      "version": "base",
      "uuid": "10c720eb-dcce-46f7-baa3-28bacef0ec0f"
    },
    {
      "status": "running",
      "name": "mgmtManaged-10.14.6-1585776660490226000",
      "stop_date": "2020-04-01T21:36:11.742662Z",
      "creation_date": "2020-04-01T21:31:01.055250Z",
      "version": "",
      "uuid": "dcbeb319-421a-4d30-8466-194eb7fa5f75"
    }
  ],
  "message": ""
}
```

## What next?

- Browse the [Anka CLI Command Reference]({{< relref "docs/Anka CLI/commands.md" >}}).  
- Connect your cloud to a [CI server]({{< relref "docs/Anka Build Cloud/CI Plugins/_index.md" >}}).  
- Find out how to use the [Controller REST API]({{< relref "docs/Anka Build Cloud/controller-api.md">}}).  
- Learn how to work with [USB devices]({{< relref "docs/Anka Build Cloud/using-real-devices-attached-to-anka-vms.md">}})


