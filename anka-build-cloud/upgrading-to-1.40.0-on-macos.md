---
title: "Upgrading to 1.40.0 on macOS"
toc_hide: true
weight: 1
---

{{< hint warning >}}
**This guide is for users deploying the Controller and Registry on macOS. There are no changes for Linux/Docker packages.**
{{< /hint >}}

Prior to 1.40.0 of the Anka Build Cloud Controller & Registry, we provided a native macOS pkg that combined the Controller, Registry, and etcd. We also provided a standalone Registry pkg, which has also drastically changed, but most users relied on the combined package.

Starting in 1.40.0, we are no longer providing the combined package and will now only provide the two standalone packages: **The Controller + etcd** and the **Registry package**.

Users coming from the combined package need to consider the appropriate migration path which we will detail below. If you are only using the Standalone Registry package, there will be instructions included at the bottom of this guide for that too.

## Combined Package

Any ENVs you had in the combined package's `/usr/local/bin/anka-controllerd` specific to the registry that *was* built-in must be moved into the registry's config at `/usr/local/bin/anka-registryd`.

#### Quick Overview of changes

- No combined pkg available. PKG will now be named `anka-controller-arm64-VERSION.pkg`.

- Any registry specific ENV flags are no longer available for the controller standalone package.

- The `ANKA_STANDALONE` ENV will now only control if the built-in `etcd` will start with the controller.

#### Steps to Upgrade

{{< hint warning >}}
You will need to re-create all of your Groups and permissions. You can technically migrate them one by one, but the time it takes to do this vs just re-creating them on the new packages is the same, if not slower.
{{< /hint >}}

1. Uninstall the current standalone package: `sudo /Library/Application\ Support/Veertu/Anka/tools/controller/uninstall.sh`

2. Follow the [setup guide]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry-mac.md" >}}).

## Standalone Registry Users

{{< hint warning >}}
Registry mac package requires a full uninstall and re-install to upgrade.
{{< /hint >}}

The Registry Standalone package for macOS now comes with an updated plist, a new `anka-registryd` file that holds configuration, and a `anka-registry` executable that has `start/restart/stop/status/logs` commands (identical to the controller mac package). [Full Registry Standalone guide here]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry-mac.md#standalone-registry-macos" >}}).

#### Quick Overview of changes

- A command is now available to control the registry: `usage: /usr/local/bin/anka-registry [start|stop|restart|status|logs]`

- Configuration is done using ENVs set in /usr/local/bin/anka-registryd (similar to the controller).

- Registry mac package logs will be at `/Library/Logs/Veertu/AnkaRegistry`.

- Registry mac package uninstaller at `/Library/Application Support/Veertu/Anka/tools/registry/uninstall.sh`.

- Mac package names have changed to be similar to our docker packaging. For example, `anka-registry-amd64-VERSION.pkg`.

- Registry mac package _was_ using port `80` by default. It is now `8089`.

