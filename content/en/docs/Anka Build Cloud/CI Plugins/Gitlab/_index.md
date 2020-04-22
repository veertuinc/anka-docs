---
date: 2019-07-03T22:24:47-05:00
title: "Integrating Gitlab and Anka Build Cloud"
linkTitle: "Gitlab"
weight: 8
description: >
  Instructions on how to use Gitlab with Anka Build Cloud
---

If you are using GitLab, Veertu provides and maintains the [Anka Gitlab Runner](https://github.com/veertuinc/gitlab-runner). The runner connects GitLab to the Anka Cloud Controller to perform Instance creation and much more.
 
## VM Template & Tag Requirements

> The below list are the absolute neccessities needed to execute commands in a VM through your CI. You may have to include other dependencies depending on your setup.

1. In the VM:
    - Install `git`
    - Make sure remote login is enabled (`System Preferences > Sharing`).
2. On the host, enable [port forwarding]({{< relref "docs/Anka CLI/command-reference.md#example---add-port-forwarding" >}}) for your VM Template using the Anka CLI.
> _We recommend not specifying `--host-port`._
3. `sudo anka suspend {VM Template name}`
4. `sudo anka registry push {VM Template name} {Tag name}`

## Install the Anka GitLab Runner

> These steps are based on GitLab 12.9.4 (9d231b25b49). Your version and UI may differ slightly.

> You can run the Anka GitLab Runner on any machine with network access to the Controller and your GitLab.

{{< include file="shared/content/en/docs/Anka Build Cloud/CI Plugins/Gitlab/partials/_download-and-install-runner.md" >}}


## Gitlab Setup

There are two methods to use the runner:

1. Set up a shared Runner
2. Set up a repo specific Runner






## Install and Configure the Anka Runner in GitLab

There are two options to activate the runner:

1. `Admin Area` > 

1. Gitlab-anka runner on docker

**1**. Go to your **project settings**→ **CI/CD** → **runner**, and you will see this screen.

![gitlab agent info](/images/gitlab-agentSpecs.png)

**2**. Run docker container with the **"anka-gitlab runner"** image, with the following command. 
Url and Token should be taken from the location of the above snapshot.
## Docker run command: 

     docker run -t asafg6/gitlab-anka-runner --executor anka
     --url http://YOUR_GITLAB_HOST --registration-token ** --ssh-user   
     VM_USER --ssh-password VM_PASSWORD --anka-controller-address
     http://ANKA_CLOUD_ADDRESS --anka-image-id IMAGE_ID --name my-anka-runner 

 * `anka-image-id` - This is the UUID of the VM template you want to use.
 * `ssh-user` - ssh user created in the VM template. You can use the default user ‘anka’ that is created
 * `ssh-password`- ssh user password created in the VM template.


## More Anka optional parameters:
    * `anka-tag`- value use a specific tag.
    * `anka-node`- id value run on a specific node.
    * `anka-priority` - value override the task's default priority.
    * `anka-group-id` - value run on a specific node group.
    * `anka-keep-alive-on-error` - Keep the VM alive in case of an error for debugging purposes.

**3**. Refresh the gitlab browser page, your runner should show under “the runner activated for this project” 


![gitlab agent info](/images/gitlabAgent-online.png)

*if the runner is on an AWS instance you will need to allow the AWS instance public IP in its own inbound rules- so the runner will be able to connect to the instance.












