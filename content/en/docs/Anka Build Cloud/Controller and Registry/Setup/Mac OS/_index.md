
---
date: 2019-07-03T22:24:47-05:00
title: "Mac OS"
linkTitle: "Mac OS"
weight: 2
description: >
  Anka Build Cloud installation instructions for Mac OS.
---

[//]: # (TODO: split this to files and add configuration )


## Controller/Registry Mac Application package
### Install
Install the `AnkaControllerRegistry-XXX.pkg` on the mac machine on which you want to install controller and Registry.  

#### Default settings  

1. Anka Controller listens on port 80
2. Anka Registry is listening on port 8089
3. Registry data is stored at `/Library/Application Support/Veertu/Anka/registry`

#### Start,Stop Controller  

Use `sudo anka-controller` command to start/stop/check status of the controller.

```
sudo anka-controller --help
usage: /usr/local/bin/anka-controller [start|stop|restart|status|logs]
```

#### Changing location of VM storage in the Registry

1. Stop the controller service `sudo anka-controller stop`
2. Change the default settings, including the location of registry storage by editing the `/usr/local/bin/anka-controllerd`.
3. Start the controller service `sudo anka-controller start`

Go to the `localhost:port` in the browser on the mac where you installed the controller/registry package and you will see the controller dashboard.  

### Uninstall

From the command line, execute the following command.

`sudo /Library/Application Support/Veertu/Anka/tools/controller/uninstall.sh`

## Debugging Issues with Controller

Anka controller is a server that usually runs as a daemon and errors are usually written to a log.
The easiest way to debug the controller is to run ‘/usr/local/bin/anka-controllerd’ directly. This will output the controller log to the screen and you will be able to see if there are any errors.
1. First stop the controller with `sudo anka-controller stop`.

2. Then execute `/usr/local/bin/anka-controllerd`.

You can also tail the log, the default location is '/Library/Logs/Veertu/AnkaController'. Check ‘/usr/local/bin/anka-controllerd’ in order to see the log dir location.