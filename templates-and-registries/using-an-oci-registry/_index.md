---
title: "Using an OCI registry"
linkTitle: "Using an OCI registry"
weight: 3
description: >
  Push and pull full and thin Anka tags with an OCI Distribution–compliant registry
aliases:
  - "/anka-virtualization-cli/working-with-oci-registries/"
  - "/anka-build-cloud/working-with-oci-registries/"
---

{{< hint error >}}
**The Anka Build Cloud Controller does not use OCI registries.** OCI registries do not work with the Controller, the Anka Registry (port `8089`), or the Registry REST API in [Working with the Registry and API]({{< relref "anka-build-cloud/working-with-registry-and-API.md" >}}). To give the Controller a template that you built from thin tags, see [Combining both registries]({{< relref "templates-and-registries/combining-both-registries.md" >}}).
{{< /hint >}}

Starting in Anka 3.9.0, the Anka CLI can push and pull VM templates to third-party **OCI Distribution–compliant** registries. Think of Anka VM templates as Docker images, and Anka tags as Docker tags. These templates are full tags: they include the macOS disk. Starting in Anka 3.10.0, an OCI registry can also store thin tags: file layers without the macOS disk. See [OCI tags]({{< relref "templates-and-registries/using-an-oci-registry/oci-tags.md" >}}).

## Prerequisites

- Anka Virtualization **3.9.0 or later** for full tags. Anka Virtualization **3.10.0 or later** for thin tags.
- Network access to your OCI registry endpoint.
- Registry credentials (username and password, or other auth your registry requires).

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

You can also skip `anka registry add` and give the registry URL to `anka push` or `anka pull`. An `oci://` URL, or an `https://` URL whose path contains `/v2/`, is an OCI registry.

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

## Push and pull full templates

Push and pull use the same commands as the Anka Registry, but traffic goes to the OCI endpoint you configured:

```shell
anka registry push -d "CI base image" -t v1 myVM
anka registry pull -t v1 myVM
```

With `push_mode` `1` (the default), the push stores the full template. See [Push modes]({{< relref "templates-and-registries/templates-and-tags.md#push-modes" >}}).

{{< hint warning >}}
If you don't use `latest` for the tag name, we create an alias that is always available and always pulls the latest tag you've pushed.
{{< /hint >}}

### Tuning push performance

On slow or high-latency links, you can increase IO buffer sizes (Anka 3.8.0+):

```bash
anka config send_buffer_size  # default 0 (curl default)
anka config recv_buffer_size
```

See [anka registry push connection aborted after 100%]({{< relref "anka-virtualization-cli/troubleshooting/cli/anka-registry-push-connection-aborted-100.md" >}}) for other push issues.

## Supported registries

Registries must be OCI compliant and support the OCI Distribution API. Veertu has tested and fixed issues for:

- Docker Hub
- Google Artifact Registry (including fixes in 3.9.2)

The sections below also have setup notes for Zot, Amazon ECR, and JFrog Artifactory. Other OCI v2 registries may work but are not guaranteed to match Anka Registry behavior for tags, metadata, or large artifact uploads.

### Zot

Zot is a simple OCI registry that can be used to store and pull Anka VM Templates.

In order to push, you need to specify the prefix `-p` to use for the VM templates.

```bash
❯ anka registry -p anka add zot http://127.0.0.1:15455
```

Once added, switch to the registry with `anka registry set zot`.

To push a VM template, you can use the `anka registry push {vm} -t {tag}` command.

Let's say I pushed a VM template with the name `26.4.1-arm64` and the tag `v1`. This will create:

```bash
❯ ./zli-darwin-arm64 --url http://127.0.0.1:15455 repo list

REPOSITORY NAME
anka/26.4.1-arm64
```

{{< hint info >}}
Ensure your config sets a long or disables the timeout or else you will get a connection timeout when pushing large templates/layers:

```json
 "http": {
    "address": "${LISTEN_ADDR}",
    "port": "${LISTEN_PORT}",
    "readTimeout": "0",
    "writeTimeout": "0"
  },
```
{{< /hint >}}

### Docker Hub

#### Pushing

{{< hint warning >}}
Docker Hub likes to ratelimit pushes, so you may get interrupted with something like:

```bash
Tue Jun 23 18:29:16 10 (reg) 71958: < docker-ratelimit-source: <IP>
Tue Jun 23 18:29:16 10 (reg) 71958: <
{"errors":[{"code":"UNAUTHORIZED","message":"authentication required","detail":[{"Type":"repository","Class":"","Name":"veertu/xxxx","Action":"pull"},{"Type":"repository","Class":"","Name":"veertu/xxxxx","Action":"push"}]}]}
...
Tue Jun 23 18:29:17 10 (reg) 71958: * client read function EOF fail, only only 0/2403 of needed bytes read
Tue Jun 23 18:29:17 10 (reg) 71958: * Connection #1 to host registry.hub.docker.com left intact
Tue Jun 23 18:29:17 40 (reg) 71958: request failed with result 26: Failed to open/read local data from file/application
```

You just need to wait and try again later if so.
{{< /hint >}}

Docker Hub is a popular registry for Docker images. Fortunately, it's ultimately OCI and we can use it to store and pull Anka VM Templates too.

```bash
❯ anka registry -p orgName -u dockerhubUser:dckr_pat_XXXXX add dockerhub https://registry.hub.docker.com
```

The prefix is the either the org or user the repository is under. The `-u` is the username and password for the registry, colon separated. You can use a personal access token (PAT) for the password.

Once added, switch to the registry with `anka registry set dockerhub`.

To push a VM template, you can use the `anka registry push {vm} -t {tag}` command.

Let's say I pushed a VM template with the name `26.4.1-arm64` and the tag `v1`. This will create a repository in Docker Hub with the name `orgName/26.4.1-arm64` and a tag with the name `v1`.

#### Pulling

To pull a VM template from the active registry configured locally, you can use the `anka pull {vm} -t {tag}` command as normal. However, for a public repository/template/tag, you need to tell it that the registry is OCI with `-o2`. Example: `anka --debug pull -o2 veertu/getting-started-templates https://registry-1.docker.io`.

### Amazon ECR public and private registries

{{< hint warning >}}
For ECR, you can use `registry add`, but you will need to refresh the credentials every ~12 hours. The credentials used for AWS eventually expire and you need to refresh them.
{{< /hint >}}

AWS ECR requires use of the AWS CLI to authenticate. You must have a recent version of the AWS CLI installed and configured with the proper credentials.

#### Public registries

An example of pulling a VM template from a public ECR registry of `public.ecr.aws/veertu` (a custom alias):

```bash
export ANKA_REGISTRY_AUTH_TOKEN=$(aws ecr-public get-authorization-token)
anka registry -o2 -p veertu -r https://public.ecr.aws pull VMNAME -t TAG
```

- The prefix (`-p`) is the "alias" seen under Public Registry > Settings.
- `-o2` is the OCI Distribution API version.
- `-r` is the registry URL.

This is the same for pushing.

#### Private registries

For private ECR registries, you can use the same approach as public registries, but you will need the private URL instead of the public URL and use ANKA_REGISTRY_USER instead of ANKA_REGISTRY_AUTH_TOKEN.

```bash
export ANKA_REGISTRY_USER="AWS:$(aws ecr get-authorization-token)"
anka registry -o2 -p veertu -r https://4585690006.dkr.ecr.us-west-2.amazonaws.com pull VMNAME -t TAG
```

- The prefix (`-p`) is the "alias" seen under Private Registry > Settings.
- `-o2` is the OCI Distribution API version.
- `-r` is the registry URL including the region.

### JFrog Artifactory

JFrog Artifactory uses the same approach as Docker Hub. See instructions above.

### Google Artifact Registry

Google Artifact Registry is a OCI compliant registry that can be used to store and pull Anka VM Templates. You must create the repository in the Google Cloud Console first as a "Docker" repository, choosing the region and a name. I like to use `anka-macos` (`{repository name}`) as my repo name and push a template that's named after the version of macOS it's for.

```bash
export ANKA_REGISTRY_AUTH_TOKEN=$(gcloud auth print-access-token)
anka registry -p {project ID}/{repository name} -r https://us-west1-docker.pkg.dev -o2 push 26.5.2-arm64 --tag vanilla --force
```

- The prefix (`-p`) is the project ID + the repository name.
- `-o2` is the OCI Distribution API version.
- `-r` is the registry URL including the project ID.
- `--force` is used to overwrite the existing VM template if it already exists.

Pulling example:

```bash
anka registry -p {project ID}/{repository name} -r https://us-west1-docker.pkg.dev -o2 pull 26.5.2-arm64 -t vanilla
```

## What OCI does not provide

Compared to the [Anka Registry and API]({{< relref "anka-build-cloud/working-with-registry-and-API.md" >}}), OCI registries do **not** offer:

- Anka Build Cloud Controller template distribution or scheduling
- Anka Registry REST API (`/registry/vm`, status endpoints, and related calls)
- Controller UI template management
- Build Cloud node join / agent pull workflows against OCI storage

## FAQs

- If you get "uuid mismatch" error, you need to add the `--remote-vm {destination repo name}` flag to the `anka registry push` command.
