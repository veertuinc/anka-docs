---
date: 2026-05-04T01:00:00-00:00
title: "Anka Virtualization 3.9.0"
---

## Ability to mount host directories inside of the VM

Starting in 3.9.0, you can share host directories with a running VM. Mounts appear under `/Volumes/My Shared Files` in the guest (this path is set by Apple and cannot be changed).

{{< hint info >}}
Host directory mounts are not currently supported on Intel.
{{< /hint >}}

Highlights:

- Multiple VMs can mount the same host folder.
- Use `anka mount` / `anka unmount` on a running VM, or `anka modify {vm} mount` / `anka modify delete mount` to persist mounts on a template.
- Mounts survive VM reboots.

For full examples and command reference, see [Sharing host directories inside the VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms/sharing-host-directories.md" >}}).

```bash
❯ anka mount test ~/
/Volumes/My Shared Files/nathanpierce

❯ anka run test bash -c "cat /Volumes/My\ Shared\ Files/nathanpierce/testfile1"
hello
```

## OCI Registry Support

Anka 3.9.0 and later can push and pull VM templates to OCI-compliant registries (for example Docker Hub and Google Artifact Registry) using the Anka CLI.

{{< hint warning >}}
OCI registries are **not** part of Anka Build Cloud. They do not work with the Controller, the Anka Registry (port `8089`), or the Registry REST API. OCI uses a different protocol and storage model than the native Anka Registry.
{{< /hint >}}

Add an OCI registry and set it as default:

```bash
❯ anka registry add my-docker https://index.docker.io -o v2 -u "username:password"
❯ anka registry set my-docker
```

Use `-p` / `--prefix` on registry commands when the remote requires a namespace prefix for image names. Authentication also supports `--api-key-id` / `--api-key` (UAK/TAP) and TLS client certs (`--cert`, `--key`, `--cacert`).

Push and pull use the same commands as the native registry:

```bash
❯ anka registry push myVM -t v1
❯ anka registry pull myVM -t v1
```

For setup details, supported targets, and limitations, see [Working with OCI Registries]({{< relref "anka-virtualization-cli/working-with-oci-registries.md" >}}).
