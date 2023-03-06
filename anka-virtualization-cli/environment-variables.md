---
date: 2019-12-12T00:00:00-00:00
title: "Environment Variables"
linkTitle: "Environment Variables"
weight: 4
description: >
  A list of available environment variables for the Anka CLI 
---

By default, all `anka config` options are available as Environment Variables by simply converting their name to uppercase and prefixing it with `ANKA_`. So, `default_user` becomes `ANKA_DEFAULT_USER` **and the ENV takes precedence**.

Outside of the config environment variables, there are a few others you might find useful:

| Name | Description |
| --- | --- |
| ANKA_REGISTRY                   | The URL of the registry to use for anka pull/push/registry commands |
| ANKA_NETWORK_MODE               | Allow setting network mode when running `anka create` |
| ANKA_NETWORK_CONTROLLER         | Allow setting network controller when running `anka create` |
| ANKA_DISK_CONTROLLER            | Allow setting disk controller when running `anka create` |
| ANKA_MACHINE_READABLE           | Whether or not commands return machine readable |
| ANKA_LOG_LEVEL                  | The log level for Anka CLI. Set to `DEBUG`, this will allow click scripts to output /tmp/_last* png files for debugging |
| ANKA_START_MODE                 | Allow starting the VM in normal or recovery mode. Recovery mode is `2`. |
| ANKA_REGISTRY_API_KEY_ID        | The UAK ID to use for registry commands. |
| ANKA_REGISTRY_API_KEY           | The UAK private key file location for registry commands |
| ANKA_HOSTNAME                   | Allows to specify VM host name on VM start |
| ANKA_BRIDGE                     | The name of the network bridge name to target. |
| ANKA_CREATE_VNC                 | Set to `0` to disable executing the VNC enable step/script in `anka create`. |
