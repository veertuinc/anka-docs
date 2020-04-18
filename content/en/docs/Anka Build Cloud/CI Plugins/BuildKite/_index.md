---
date: 2019-07-03T22:24:47-05:00
title: "Integrating BuildKite and Anka Build Cloud"
linkTitle: "BuildKite"
weight: 9
description: >
  Instructions on how to use BuildKite with Anka Build Cloud
---

## OVERVIEW

If you are using Buildkite CI as your CI tool, you need to use [Anka-Buildkite plugin](https://github.com/chef/anka-buildkite-plugin).


* If you are new to Buildkite - you should know anka VM run from Buildkite build will not show started instances on the controller dashboard or through [`anka list`]({{< relref "docs/Anka CLI/command-reference.md#list" >}}). The communication Between Buildkite and Anka takes place directly on to the ankactl (the part of anka responsible for Anka CLI and Anka run command) via the Buildkite agent . So, you will be able to see the logs activity only in [`anka.log`](https://ankadocs.veertu.com/docs/anka-build-cloud/logs/maclogs/) file.

## Connect the Buildkite-agent to the Anka host:

**1**. Install and run the Buildkite agent on your host ( run the agent via the user who owns the VM). Once the agent is running you should see your Host in the agents list on the Buildkite dashboard.

**2**. Buildkite plugins act as an extranl repository of hooks that are referenced from your pipeline.yml file. Therefore you must run your script via `.yaml` .

**3**. When you start a new pipeline project, take note of the following:

![buildkite new project screen](/images/anka-buildkite/newbuildkiteprojectscreen.png)

 3.1. The plugin does not support Buildkite environment variables.

 3.2. After the project is created you need to insert your build script as a .yaml file. Before your first build, go to **pipeline settings** → **convert to yaml**. 

 ![buildkite new project screen](/images/anka-buildkite/pipeline-settings.png)

3.3. The performance of using shared folders is not completely optimized, so it is best practice to disable this. As an alternative, it is suggested to clone and pull the repository as the first commands in the pipeline step. You can see the commands in this snapshot.

## Example using Anka plugin

    * steps:
      commands:
      - git clone $BUILDKITE_REPO && cd repo-folder && git checkout -f $BUILDKITE_COMMIT  //clone and pull the repository. 
      - cd repo-folder; ./build.sh // build the code.  
    plugins:
      - thedyrt/skip-checkout#v0.1.1:   
      - chef/anka#v0.6.0:              //anka plugin
          vm-name: base-vm-mojave  // your vm name 
          no-volume: true        // anak-plugin config option
          wait-network: true     // anka-plugin config option *link to the full list at the buttom of the page 

**Key Observations on the Plugin :**

1. By default, cloned VMs will be deleted on pipeline cancellation, failure, or success.

2. A lock file (`/tmp/anka-buildkite-plugin-lock` ) is created around pull and cloning. This prevents collision/ram state corruption when you're running two different jobs and pulling two different tags on the same Anka node. The error you'd see otherwise is `state_lib/b026f71c-7675-11e9-8883-f01898ec0a5d.ank: failed to open image, error 2` . 

There is more [configuration](https://github.com/chef/anka-buildkite-plugin) for the plugin which can be set in the `pipeline.yml`.


License
Author:
Tom Duffield (tom@chef.io)
Copyright:
Copyright 2018, Chef Software, Inc.
License:
Apache License, Version 2.0







