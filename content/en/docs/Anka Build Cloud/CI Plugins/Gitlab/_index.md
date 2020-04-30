---
date: 2019-07-03T22:24:47-05:00
title: "Integrating Gitlab and Anka Build Cloud"
linkTitle: "Gitlab"
weight: 8
description: >
  Instructions on how to use Gitlab with Anka Build Cloud
---

If you are using GitLab, Veertu provides and maintains the [Anka Gitlab Runner](https://github.com/veertuinc/gitlab-runner). The runner connects GitLab to the Anka Cloud Controller to perform Instance creation and execution through SSH.
 
## VM Template & Tag Requirements

> The below list are the absolute neccessities needed to execute commands in a VM through your CI. You may have to include other dependencies depending on your setup.

1. In the VM:
    - Install `git`
    - Make sure remote login is enabled (`System Preferences > Sharing`).
2. On the host, enable [port forwarding]({{< relref "docs/Anka CLI/command-reference.md#example---add-port-forwarding" >}}) for your VM Template using the Anka CLI. _We recommend not specifying `--host-port`._
3. `sudo anka suspend {VM Template name}`
4. `sudo anka registry push {VM Template name} {Tag name}`

## Install the Anka GitLab Runner

> These steps are based on GitLab 12.9.4 (9d231b25b49). Your version/UI may differ slightly.

> You can run the Anka GitLab Runner on any machine with network access to the Anka Cloud Controller, Anka Nodes, and your GitLab.

{{< include file="shared/content/en/docs/Anka Build Cloud/CI Plugins/Gitlab/partials/_download-and-install-runner.md" >}}

## Gitlab Setup

Once the runner is installed, you can set it up as:

1. Set up a **Shared Runner**
2. Set up a repo **Specific Runner**
3. Both

However, all of those require a Registration Token. Here are the locations to find this:

1. **Shared Runner**: Admin Area (/admin) > Overview > Runners (/admin/runners)
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

Ensure that GitLab now shows **just now** under the **Last contact** column.

---

## Common Questions

- We've defaulted retries to 0, but you can modify this using:

    ```
    --preparation-retries value           Set the amount of preparation retries for a job (default: "0") [$PREPARATION_RETRIES]
    ```
- We've stripped out everything but the anka and ssh executors. Fortunately, the `anka-gitlab-runner` is independent and doesn't conflict with the regular `gitlab-runner` if they're running on the same machine. The options that aren't associated with the stripped out executors you should still be able to use (cache-s3, maximum-timeout, etc).
- We use slightly modified versions of the original `gitlab-runner` tests (they didn't pass when we cloned 12.10.0).
- If your controller goes down the runner will retry the HTTP calls for several minutes until finally throwing an error.
