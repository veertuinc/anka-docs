---
date: 2026-05-04T00:00:00-00:00
title: "Working with OCI Registries"
linkTitle: "OCI Registries"
weight: 6
description: >
  Push and pull Anka VM templates to OCI Distribution–compliant registries using the Anka CLI
---

{{< hint error >}}
**OCI registries are not part of Anka Build Cloud.** They do not work with the Anka Build Cloud Controller, the Anka Registry (port `8089`), or the Registry REST API documented in [Working with the Registry and API]({{< relref "anka-build-cloud/working-with-registry-and-API.md" >}}). If you use Build Cloud, use the native Anka Registry only.
{{< /hint >}}

Starting in Anka 3.9.0, the Anka CLI can push and pull VM templates to third-party **OCI Distribution–compliant** registries (for example [Docker Hub](https://hub.docker.com/) and [Google Artifact Registry](https://cloud.google.com/artifact-registry)). This is a **standalone CLI workflow** for teams that want to store templates in an existing container registry instead of an Anka Registry.

OCI storage uses the OCI image format and registry APIs. It is separate from the Anka Registry protocol, template metadata APIs, and Controller integration. Do not expect Controller scheduling, Registry UI features, or the REST endpoints in the Build Cloud registry guide to apply to OCI-backed templates.

## Prerequisites

- Anka Virtualization **3.9.0 or later** on the host running `anka registry` commands
- Network access to your OCI registry endpoint
- Registry credentials (username/password, or other auth your registry requires)

{{< hint info >}}
Host directory mounts inside VMs are documented separately in [Sharing host directories inside the VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms/sharing-host-directories.md" >}}).
{{< /hint >}}

## Add an OCI registry

Use `anka registry add` with `-o` / `--oci-version` (typically `v2`) and authentication:

```shell
anka registry add my-docker https://index.docker.io -o v2 -u "username:password"
anka registry set my-docker
```

List configured registries (replaces `anka registry list-repos` in 3.9.1 and later):

```shell
anka registry --list
```

### Registry options

See [`anka registry --help`]({{< relref "anka-virtualization-cli/command-line-reference.md#registry" >}}) for the full option list. Common OCI-specific flags on `anka registry add` and `anka registry set`:

| Option | Purpose |
|--------|---------|
| `-o` / `--oci-version` | OCI Distribution API version (use `v2`) |
| `-p` / `--prefix` | Namespace prefix prepended to VM template names in the remote registry |
| `-u` / `--user` | Username and password |
| `--api-key-id` / `--api-key` | UAK/TAP authentication |
| `--cert`, `--key`, `--cacert` | TLS client certificate authentication |
| `--insecure` | Skip TLS verification (not recommended for production) |

## Push and pull

Push and pull use the same `anka registry` subcommands as the native Anka Registry, but traffic goes to the OCI endpoint you configured:

```shell
anka registry push -d "CI base image" -t v1 myVM
anka registry pull -t v1 myVM
```

### Tuning push performance

On slow or high-latency links, you can increase IO buffer sizes (Anka 3.8.0+):

```bash
anka config send_buffer_size  # default 0 (curl default)
anka config recv_buffer_size
```

See [anka registry push connection aborted after 100%]({{< relref "anka-virtualization-cli/troubleshooting/cli/anka-registry-push-connection-aborted-100.md" >}}) for other push issues.

## Supported targets

Anka 3.9.0+ supports OCI-compliant registries in general. Veertu has tested and fixed issues for:

- Docker Hub
- Google Artifact Registry (including fixes in 3.9.2)

Other OCI v2 registries may work but are not guaranteed to match Anka Registry behavior for tags, metadata, or large artifact uploads.

## What OCI does not provide

Compared to the [native Anka Registry and API]({{< relref "anka-build-cloud/working-with-registry-and-API.md" >}}), OCI registries do **not** offer:

- Anka Build Cloud Controller template distribution or scheduling
- Anka Registry REST API (`/registry/vm`, status endpoints, and related calls)
- Controller UI template management
- Build Cloud node join / agent pull workflows against OCI storage

Use the native Anka Registry for any Build Cloud or Controller workflow. Use OCI only when you intentionally store templates in a third-party registry via the CLI.

## Related

- [Anka Virtualization 3.9.0 — OCI Registry Support]({{< relref "whats-new/anka-3.9.0/index.md#oci-registry-support" >}})
- [`anka registry` command reference]({{< relref "anka-virtualization-cli/command-line-reference.md#registry" >}})
