---
date: 2019-07-03T22:24:47-05:00
title: "Using GitLab and Anka Build Cloud"
linkTitle: "GitLab"
weight: 8
description: >
  Instructions on how to use GitLab with Anka Build Cloud
---

If you are using GitLab, Veertu provides and maintains the [Anka GitLab Runner](https://github.com/veertuinc/gitlab-runner). The runner connects GitLab to the Anka Cloud Controller to perform Instance creation and execution through SSH.
 
## VM Template & Tag Requirements

> The below list is the absolute neccessities needed to execute commands in a VM through your CI and the GitLab Runner. You may have to include other dependencies depending on your needs.

1. In the VM:
    - Install `git`
    - Make sure remote login is enabled (`System Preferences > Sharing`).
2. On the host, enable [port forwarding]({{< relref "docs/Anka CLI/command-reference.md#example---add-port-forwarding" >}}) for your VM Template using the Anka CLI. _We recommend not specifying `--host-port`._
3. `sudo anka suspend {VM Template name}`
4. `sudo anka registry push {VM Template name} {Tag name}`

## Install the Anka GitLab Runner

> These steps are based on GitLab 12.9.4 (9d231b25b49); Your version/UI may differ slightly.

> You can run the Anka GitLab Runner on any machine with network access to the Anka Cloud Controller, Anka Nodes, and your GitLab.

{{< include file="shared/content/en/docs/Anka Build Cloud/CI Plugins/GitLab/partials/_download-and-install-runner.md" >}}

## GitLab Setup

Once the runner is installed, you can set it up as:

1. A **Shared Runner**
2. A repo's **Specific Runner**
3. Both

However, all of those require a Registration Token. Here are the locations to find it:

1. **Shared Runner**: Admin Area > Overview > Runners (/admin/runners)
2. **Specific Runner**: Under your repo > Settings > CI / CD > Click on Expand next to Runners

### Registration

With the token, you can now register using `anka-gitlab-runner register` (guided prompts) or with `--non-interactive`:

```shell
anka-gitlab-runner register --non-interactive \
--url "http://anka-gitlab-ce:8084/" \
--registration-token nHKqG3sYV4B5roRK1ZhW \
--ssh-user anka \
--ssh-password admin \
--name "localhost shared runner" \
--anka-controller-address "http://my.controller.url" \
--anka-template-uuid d09f2a1a-e621-463d-8dfd-8ce9ba9f4160 \
--anka-tag base:port-forward-22:brew-git:gitlab \
--executor anka \
--tag-list "localhost-shared,localhost,iOS"
```

You can find all of the options available with `anka-gitlab-runner register --help`.

> To unregister, you can use `anka-gitlab-runner unregister -n "localhost shared runner"`

You should now see the runner in your GitLab:

![gitlab runner attached](/images/gitlab-runner-attached.png)

You can now start the anka-gitlab-runner: 

```shell
❯ anka-gitlab-runner start          
Runtime platform                                    arch=amd64 os=darwin pid=36048 revision=66bf0b24 version=12.10.0/1.0-rc1
executed                                           
```

Ensure that GitLab shows **just now** under the **Last contact** column.

## Using our Docker images 

[Docker Hub - i386 Repo](https://hub.docker.com/repository/docker/veertu/anka-gitlab-runner-i386)

[Docker Hub - amd64 Repo](https://hub.docker.com/repository/docker/veertu/anka-gitlab-runner-i386)

When executing `docker run`, any arguments included are used as options for `anka-gitlab-runner register --non-interactive`:

```shell
❯ docker run -ti --rm veertu/anka-gitlab-runner-amd64 --help
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Runtime platform                                    arch=amd64 os=linux pid=693 revision=6c1a9836 version=12.10.0/1.0-rc1
NAME:
   anka-gitlab-runner register - register a new runner

USAGE:
   anka-gitlab-runner register [command options] [arguments...]

OPTIONS:
   -c value, --config value . . .
```

> We run `update-ca-certificates` each time you start the container. You can add a volume to mount in your certificates if needed.

You use the same non-interactive arguments that we mentioned in [the Registration section]({{< relref "docs/Anka Build Cloud/CI Plugins/GitLab/_index.md#registration" >}}) when executing the binary (but without `--non-interactive`):

```shell
❯ docker run -ti --rm veertu/anka-gitlab-runner-amd64 --url "http://anka-gitlab-ce:8084/" --registration-token nHKqG3sYV4B5roRK1ZhW --ssh-user anka --ssh-password admin --name "localhost shared runner" --anka-controller-address "https://anka.controller:8080/" --anka-template-uuid d09f2a1a-e621-463d-8dfd-8ce9ba9f4160 --anka-tag base:port-forward-22:brew-git:gitlab --executor anka --anka-root-ca-path /Users/nathanpierce/anka-ca-crt.pem --anka-cert-path /Users/nathanpierce/anka-gitlab-crt.pem --anka-key-path /Users/nathanpierce/anka-gitlab-key.pem --clone-url "http://anka-gitlab-ce:8084" --tag-list "localhost-shared,localhost,iOS"
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Runtime platform                                    arch=amd64 os=linux pid=693 revision=7b532a90 version=12.10.0/1.0-rc1
Running in system-mode.                            
                                                   
Registering runner... succeeded                     runner=nHKqG3sY
Updated:  /etc/gitlab-runner/anka-config.toml      
Feel free to start anka-gitlab-runner, but if it's running already the config should be automatically reloaded! 
Runtime platform                                    arch=amd64 os=linux pid=705 revision=7b532a90 version=12.10.0/1.0-rc1
Starting multi-runner from /etc/gitlab-runner/anka-config.toml...  builds=0
Running in system-mode.                            
                                                   
Configuration loaded                                builds=0
listen_address not defined, metrics & debug endpoints disabled  builds=0
[session_server].listen_address not defined, session endpoints disabled  builds=0
```

When you stop or exit the container, it will automatically unregister it from your GitLab.

---

## Common Questions

- We've defaulted preparation retries to 2, but you can modify this using:

    ```
    --preparation-retries value           Set the amount of preparation retries for a job (default: "0") [$PREPARATION_RETRIES]
    ```
- We've stripped out everything but the anka and ssh executors. Fortunately, the `anka-gitlab-runner` is independent and doesn't conflict with the regular `gitlab-runner` if they're running on the same machine. 
  - The options that aren't associated with the stripped out executors you should still be able to use (cache-s3, maximum-timeout, etc).
- We use slightly modified versions of the original `gitlab-runner` tests (they didn't pass when we cloned 12.10.0).
- If your controller goes down, the runner will retry the HTTP calls for several minutes until finally throwing an error.
