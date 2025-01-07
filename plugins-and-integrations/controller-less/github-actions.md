---
date: 2019-07-03T22:24:47-05:00
title: "Using GitHub Actions and Anka Build"
linkTitle: "GitHub Actions"
weight: 1
description: >
  Instructions on how to use GitHub Actions with the Anka Build and macOS VMs
aliases:
  - /ci-plugins-and-integrations/controller-less/github-actions/
---

## Anklet: GitHub Actions Plugins

You can now use the [Anklet](https://github.com/veertuinc/anklet) GitHub Action plugins to run on-demand and ephemeral Anka macOS VMs in GitHub Actions.

---

### Older Legacy GitHub Actions (not recommended)

{{< hint warning >}}
The github runner does not have an ARM version available but you can run the x64 without issue through Apple's rosetta.
{{< /hint >}}

The [Anka VM GitHub Action](https://github.com/marketplace/actions/anka-vm-github-action) provides a quick way to integrate Anka with GitHub Actions. The plugin helps your [self-hosted GitHub runners](https://help.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners) prepare Anka VM instances for building, testing, and more. Keep in mind that it works differently than our other plugins or integrations for Anka Build Cloud. It will not start VM instances using your Anka Build Cloud Controller and instead connect directly to an unused Node to prepare the VM and execute commands.

## VM Template & Tag Requirements

1. There are no current requirements

## Install and Configure GitHub Self-Hosted Runners

1. You'll need to [add a self-hosted runner to your project repo or organization (shared)](https://help.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners).
> Setup a runner per VM you wish to run. So, for example, if you're planning on running a maximum of two VMs on your node, you'd need two self-hosted runners.
2. Once you've confirmed that you can see the runner as Idle in GitHub, it's ready to start processing jobs.

## Pipeline Step Definition

An example workflow .yml can be found in the [Anka VM GitHub Action README](https://github.com/marketplace/actions/anka-vm-github-action).