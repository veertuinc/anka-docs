---
title: "Templates and Registries"
linkTitle: "Templates and Registries"
weight: 3
description: >
  Store and share Anka VM templates in the Anka Registry, in your own OCI registry, or in both
---

After you create and prepare a VM, you push it to a registry as a template. Other hosts then pull that template. Anka supports two types of registry. You can use one of them, or both together.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![The Anka Registry, your OCI registry, and both together]({{< siteurl >}}images/templates-and-registries/templates-and-registries.webp)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Choose a path

### Use the Anka Registry

The Anka Registry is part of [Anka Build Cloud]({{< relref "anka-build-cloud/_index.md" >}}). It stores full VM templates. When you pull a tag, the VM goes back to the exact state of that tag.

Use the Anka Registry when:

- You use the Anka Build Cloud Controller to start VMs. The Controller pulls templates only from the Anka Registry.
- You manage templates in the Controller UI or with the Registry REST API.

Go to [Using the Anka Registry]({{< relref "templates-and-registries/using-the-anka-registry.md" >}}).

### Use your OCI registry

You can use a registry that you already have, such as Docker Hub, Google Artifact Registry, Amazon ECR, JFrog Artifactory, or Zot. You use it from the Anka CLI only.

An OCI registry stores two types of tags:

- **Full templates** (Anka 3.9.0 and later). These are the same as the templates in the Anka Registry.
- **OCI tags** (Anka 3.10.0 and later). An OCI tag holds only the files that you add to a VM, such as the files for one tool like Xcode. You can pull an OCI tag onto any VM. OCI tags stack on the VM in the order that you pull them.

{{< hint info >}}
Homebrew packages are also OCI tags. You can pull a Homebrew package, such as `cmake`, onto a VM without Homebrew installed in the VM.
{{< /hint >}}

Use an OCI registry when:

- You do not use Anka Build Cloud.
- Your company already has a container registry with its own access control.
- You want to add each tool to a VM as a separate layer, or pull public OCI artifacts such as Homebrew bottles.

Go to [Using an OCI registry]({{< relref "templates-and-registries/using-an-oci-registry/_index.md" >}}).

### Use both registries

Keep the macOS base template in the Anka Registry, and keep each tool as an OCI tag in an OCI registry. A producer host pulls the OCI tags onto the base template and pushes the finished VM to the Anka Registry. The Build Cloud Controller can then start VMs from that template.

Go to [Combining both registries]({{< relref "templates-and-registries/combining-both-registries.md" >}}).

### Already use the Anka Registry?

You can move to OCI tags one step at a time, and keep your current templates. Go to [Moving to OCI tags]({{< relref "templates-and-registries/moving-to-oci-tags.md" >}}).

## Compare the registries

| Feature | Anka Registry | OCI registry |
|---------|---------------|--------------|
| Anka Build Cloud Controller | Yes | No |
| Controller UI and Registry REST API | Yes | No |
| Full templates | Yes | Yes, Anka 3.9.0 and later |
| OCI tags (file layers) | No | Yes, Anka 3.10.0 and later |
| `--thin` and `push_mode` | Not used | Used |
| Public OCI artifacts, such as Homebrew bottles | No | Yes |
| Server | You run the Anka Registry on Linux, Docker, or macOS | Your existing registry, or a hosted service |

Before you choose, read [Templates and tags]({{< relref "templates-and-registries/templates-and-tags.md" >}}). It explains how full tags and OCI tags behave on a VM.
