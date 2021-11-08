---
date: 2019-07-03T22:24:47-05:00
title: "Using Buildkite and Anka Build"
linkTitle: "Buildkite"
weight: 1
description: >
  Instructions on how to use Buildkite with Anka Build
---

The [anka-buildkite-plugin](https://github.com/chef/anka-buildkite-plugin) provides a quick way to integrate Anka with Buidkite. The plugin helps Buildkite jobs dynamically provision Anka VM instances for building, testing, and more.

The Anka Buildkite plugin works differently than our other plugins. It will not start VM instances using your Anka Build Cloud Controller and instead connect directly to an unused Node agent to execute the VM setup commands.

## VM Template & Tag Requirements

1. None

## Install and Configure the Anka Buildkite Plugin

1. You'll need to [install the buldkite agent onto your Nodes](https://buildkite.com/docs/agent/v3/osx).
  {{< hint info >}}
  You have the ability to set the `spawn` count inside of your `buildkite-agent.cfg`. This will allow two VMs to run at once on a single Node.
  {{< /hint >}}
2. Once you've confirmed that you can see the Agents/Nodes in your Buildkite Org, you're ready to setup a pipeline.

## Pipeline Step Definition

Buildkite provides [manual or YAML options](https://buildkite.com/docs/pipelines/command-step) when defining pipeline steps. An example on setting up the `anka-buildkite-plugin` using a YAML file in your project's repo can be found in the [plugin's Github README](https://github.com/veertuinc/anka-buildkite-plugin).

---

We'd like to recognize Tom Duffield (tom@chef.io) for developing the plugin for Anka. The official repo can be found [HERE](https://github.com/chef/anka-buildkite-plugin).