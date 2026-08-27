---
date: 2026-08-25T00:00:00-00:00
title: "macOS runners for a CI service provider"
linkTitle: "CI service provider"
weight: 3
description: >
  Multi-tenant macOS GitHub-style runners with CLI-only Anka and custom orchestration
---

## Problem

Bare-metal hosts are a security problem in a multi-tenant runner service. Customer jobs are untrusted and unpredictable. One job might compile for a minute; the next might run a long test suite with network access. A customer job that runs on the node can read other customers’ IP, leftover artifacts, and host credentials. Persistent VMs had the same leak: files, keys, and network reachability stayed between customers.

Security is critical. Tenant isolation cannot depend on trust or manual cleanup between jobs.

What is needed:

- A VM that exists only for one job, then is gone.
- Isolation so tenant A cannot reach tenant B’s VM, tenant B’s IP, or the host.
- Orchestration path to clone, start, and terminate VMs on each host.
- A small template set that still covers many Xcode and toolchain versions customers ask for.
- Enough VMs per host to make Apple Silicon hardware pay for itself.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Bare metal host risks versus isolated VMs]({{< siteurl >}}images/use-cases/ci-service-provider/bare-metal-risk.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Deployment model

A large Mac node pool in the provider’s data centers. Each host runs the [Anka Virtualization CLI]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}}) and the provider’s orchestrator. There is no Anka Controller or Registry in this setup.

Golden templates live in Azure Blob Storage. The platform team builds a template once, runs [`anka export`]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md" >}}), and uploads the archive. Host provisioning uses `azcopy` to fetch the archive, then [`anka import`]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md" >}}) loads the template locally. When a job arrives, the orchestrator clones from that imported template, starts the VM, and deletes it when the job ends.

{{< hint info >}}
Customers never SSH to Mac nodes or run Anka CLI commands. The orchestrator is the only path to clone, start, and stop VMs. Internal image work happens on a separate build host before export to Azure.
{{< /hint >}}

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Azure template storage, host import, and custom orchestrator]({{< siteurl >}}images/use-cases/ci-service-provider/deployment-orchestration.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Scale

<!-- REVIEW: scale -->
Hundreds of concurrent VMs. Node count is lower than VM count because each host runs several VMs when the job size allows it. Heavier customer jobs run in a VM configured to consume the full resources of the host. Create and delete rate stays high all day.

Template count stays small: a handful of macOS templates, each with one tag. The provider does not build a unique image per customer or per Xcode version.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Two VMs per node versus one full-host VM by job size]({{< siteurl >}}images/use-cases/shared/release-scale.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## How Anka is used

### Mac fleet and orchestrator

Each Mac node has the [Anka CLI]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}}) installed. The provider’s orchestrator schedules jobs across the pool, picks a host, clones from the imported base template on that host, starts the VM, streams logs back to the CI product, and terminates the VM when the job finishes or times out.

### Templates: one tag, many tool versions

The platform team maintains a handful of [VM templates]({{< relref "anka-virtualization-cli/getting-started/creating-vms/_index.md" >}}). Each template has one tag. That tag is packed with multiple Xcode versions plus other stacks customers need, installed side by side and managed with version managers (`nvm`, and similar tools).

Customers pick a runner type in the product. At the start of CI they run a provider command to select the Xcode or toolchain version for that job. The provider does not publish a separate template per version combination.

When the platform team refreshes a template, they export it, upload to Azure, and roll the new archive out to hosts through their import pipeline.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![One template tag with multiple Xcode and toolchain versions]({{< siteurl >}}images/use-cases/ci-service-provider/template-model.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

### CI and jobs

1. A customer job arrives. The orchestrator picks a host with the imported template already on disk.
2. The orchestrator clones from the local template and starts the VM.
3. The job runs the version-switch command, then the customer build over SSH or the provider’s agent inside the VM.
4. The orchestrator terminates and deletes the VM. The next customer does not inherit disk or network state.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![Orchestrator clone, start, and terminate job flow]({{< siteurl >}}images/use-cases/ci-service-provider/job-lifecycle.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

Runner templates use [CLI advanced security]({{< relref "anka-virtualization-cli/advanced-security-features.md" >}}): `--no-local` and [IP filtering]({{< relref "anka-virtualization-cli/getting-started/understanding-vm-networking.md#ip-filtering" >}}) (`block local`). Host tools, Azure credentials, and other tenants’ VMs stay unreachable from inside the job VM. That matters when customer workloads vary this much and you cannot predict what a job will try to reach.

{{< rawhtml >}}<center>{{< /rawhtml >}}
![VM network isolation with block local and no-local]({{< siteurl >}}images/use-cases/ci-service-provider/network-isolation.svg)
{{< rawhtml >}}</center>{{< /rawhtml >}}

## Value

<!-- REVIEW: value -->
Hosts stay busy because a finished job frees the VM slot instead of leaving a dirty machine. Isolation is a product feature, not a runbook. Ops work moves to template export and Azure rollout, not per-host manual image prep.

A handful of fat templates cover many customer toolchain requests without a matrix of Registry tags or Controller SKUs. Export and import through Azure keeps every host on the same golden image without a central Anka Registry pull on every job start.

## Practices that worked

- Keep the template list small. Pack multiple Xcode and toolchain versions into one tag; let customers switch at job start.
- Own orchestration end to end. Do not expose Anka CLI or host access to customers.
- Never schedule a customer job on the bare-metal host. Host access is how other people’s IP and your node credentials leak.
- Set `--no-local` or IP filter `block local` on every runner template. Diverse GitHub-style workloads make network isolation non-optional.
- Use `anka export` and `anka import` plus object storage (`azcopy` to Azure) to roll templates to hosts. Treat the blob archive as the source of truth.
- Set job timeouts so a hung customer build terminates the VM and frees the slot.
- Measure clone time and slot occupancy. Cold imports and underfilled nodes are the usual density leaks.

## Related docs

- [Getting Started (CLI)]({{< relref "anka-virtualization-cli/getting-started/_index.md" >}})
- [Creating VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms/_index.md" >}}) (includes export and import)
- [Installing the Anka Virtualization package]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}})
- [Advanced Security Features (CLI)]({{< relref "anka-virtualization-cli/advanced-security-features.md" >}}) (`--no-local`, IP filtering)
- [VM networking]({{< relref "anka-virtualization-cli/getting-started/understanding-vm-networking.md" >}})
