---
date: 2019-07-03T22:24:47-05:00
title: "Integrating Gitlab and Anka Build Cloud"
linkTitle: "Gitlab"
weight: 8
description: >
  Instructions on how to use Gitlab with Anka Build Cloud
---


### OVERVIEW
If you are using Gitlab CI as your CI tool, Veertu provides and maintains a [Gitlab CI Runner](https://github.com/veertuinc/gitlab-runner) for Anka Build. The runner connects Gitlab to anka controller and builds your project upon the VM . You can run the runner on any machine.
 
## Prepare VM Template: 

1. Start or create an Anka VM. Install git and your build/test dependencies 
2. Suspend the VM
3. If you haven't changed your vm default networking, configure port forwarding for ssh: anka modify $VM_NAME add port-forwarding --guest-port 2--host-port 0 ssh
4. Push the VM to the registry.

Now you have a base image to use for GitLab CI.	

**There are two options to activate the runner.** 

## Option 1 - Gitlab-anka runner on docker

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

*if the runner is on an AWS instance you will need to allow its public IP in its inbound rules- so the runner will be able to connect to the instance***












