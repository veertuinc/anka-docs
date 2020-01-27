---
date: 2019-07-26
title: "Preparing Mac Host as Anka Node"
linkTitle: "Preparing Mac Host as Anka Node"
weight: 3
description: >
  Preparing Mac Host as Anka Node
---
[//]: # (TODO: split this into files)


## Configuration of Mac Host Machine as Anka Node

Anka agent, which is included in `anka.pkg` is installed as part of anka binary installation on the mac machines. These are the mac host on which the Anka macOS VMs run. Do the following on the mac hosts.

Anka package `anka.pkg` can be installed on mac machines running Hi Sierra(10.13.x) or Higher.

1. Go to Preferences, Users and enable automatic login for the user.

2. Go to Preferences, Security and check `Remove the need to require a password` option.

3. Go to Preferences, Energy Saver and check on `Disable sleep`

4. Install the `anka.pkg` mac application package on the machine.


