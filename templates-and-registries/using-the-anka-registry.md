---
title: "Using the Anka Registry"
linkTitle: "Using the Anka Registry"
weight: 2
description: >
  Push and pull full VM templates with the Anka Registry from Anka Build Cloud
---

The Anka Registry is part of Anka Build Cloud. It stores full VM templates, and the Build Cloud Controller pulls templates from it. To install the Anka Registry, see [Setting up the Controller & Registry on Linux/Docker]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) or [on macOS]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry-mac.md" >}}).

## Add the registry

{{< include file="_partials/anka-virtualization-cli/getting-started/_add-the-registry.md" >}}

## Push a template

`anka push` sends the tag to the registry in `ANKA_REGISTRY`. To use a different registry, add the registry name or URL.

```bash
❯ anka push 27.0.1 -t vanilla
❯ anka push 27.0.1 -t vanilla http://anka.registry:8089
```

The Anka Registry always stores the full tag. `--thin` and `push_mode` do not apply.

## Pull a template

```bash
❯ anka pull 27.0.1 -t vanilla
```

When you pull a tag, the VM goes back to the exact state of that tag.

## Manage templates and tags

- [Registry VM Templates and Tags]({{< relref "anka-build-cloud/getting-started/registry-vm-templates-and-tags.md" >}}) describes how to replace a template and keep its name and UUID.
- [Working with the Registry and API]({{< relref "anka-build-cloud/working-with-registry-and-API.md" >}}) describes all `anka registry` commands and the Registry REST API.

## Add thin tags to your templates

You can add tools from thin tags in an OCI registry to a template in the Anka Registry. See [Combining both registries]({{< relref "templates-and-registries/combining-both-registries.md" >}}). To move your current templates step by step, see [Moving to OCI tags]({{< relref "templates-and-registries/moving-to-oci-tags.md" >}}).
