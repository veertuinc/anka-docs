


In 3.9.0 we've added support for OCI registries. This allows you to store and pull VM templates from OCI registries like:

1. ZOT (or any other strictly OCI compliant registry)
2. Dockerhub
3. ECR Public and Private Registries
4. Jfrog Artifactory

Think of Anka VM Templates as Docker Images and Anka VM Tags as Docker Tags.

{{< hint warning >}}
OCI registries are not currently compatible with the Anka Controller/Build Cloud. We will be adding support for this in a future release.
{{< /hint >}}

{{< hint info >}}
Registries must be OCI compliant and support the OCI Distribution API.
{{< /hint >}}

{{< hint warning >}}
If you don't use `latest` for the tag name, we create an alias that is always available and always pulls the latest tag you've pushed.
{{< /hint >}}

### ZOT

ZOT is a simple OCI registry that can be used to store and pull Anka VM Templates.

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

### Dockerhub

Dockerhub is a popular registry for Docker images. Fortunately, it's ultimately OCI and we can use it to store and pull Anka VM Templates too.

```bash
❯ anka registry -p dockerhubUser -u dockerhubUser:dckr_pat_XXXXX add dockerhub https://registry-1.docker.io
```

The prefix is the either the org or user the repository is under. The `-u` is the username and password for the registry, colon separated. You can use a personal access token (PAT) for the password.

Once added, switch to the registry with `anka registry set dockerhub`.

To push a VM template, you can use the `anka registry push {vm} -t {tag}` command.

Let's say I pushed a VM template with the name `26.4.1-arm64` and the tag `v1`. This will create a repository in Dockerhub with the name `dockerhubUser/26.4.1-arm64` and a tag with the name `v1`.

### ECR Public and Private Registries

{{< hint warning >}}
For ECR, you can use `registry add`, but you will need to refresh the credentials every ~12 hours. The credentials used for AWS eventually expire and you need to refresh them.
{{< /hint >}}

AWS ECR requires use of the AWS CLI to authenticate. You must have a recent version of the AWS CLI installed and configured with the proper credentials.

#### Public Registries

An example of pulling a VM template from a public ECR registry of `public.ecr.aws/veertu` (a custom alias):

```bash
export ANKA_REGISTRY_AUTH_TOKEN=$(aws ecr-public get-authorization-token)
anka registry -o2 -p veertu -r https://public.ecr.aws pull VMNAME -t TAG
```

- The prefix (`-p`) is the "alias" seen under Public Registry > Settings.
- `-o2` is the OCI Distribution API version.
- `-r` is the registry URL.

This is the same for pushing.

#### Private Registries

For private ECR registries, you can use the same approach as public registries, but you will need the private URL instead of the public URL and use ANKA_REGISTRY_USER instead of ANKA_REGISTRY_AUTH_TOKEN.

```bash
export ANKA_REGISTRY_USER="AWS:$(aws ecr get-authorization-token)"
anka registry -o2 -p veertu -r https://4585690006.dkr.ecr.us-west-2.amazonaws.com pull VMNAME -t TAG
```

- The prefix (`-p`) is the "alias" seen under Private Registry > Settings.
- `-o2` is the OCI Distribution API version.
- `-r` is the registry URL.

### Jfrog Artifactory

Jfrog's artifactory uses the same approach as Dockerhub. See instructions above.
