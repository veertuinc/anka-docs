---
date: 2019-07-03T22:24:47-05:00
title: "Using Jenkins and the Anka Build Cloud"
linkTitle: "Jenkins"
weight: 1
description: >
  Instructions on how to use Jenkins with Anka Build Cloud
aliases:
  - /ci-plugins-and-integrations/controller-+-registry/jenkins/
  - "docs/plugins-and-integrations/jenkins/"
---

The Jenkins **Anka Plugin** provides a quick way to integrate Anka Build Cloud with Jenkins. The plugin helps Jenkins jobs dynamically provision Anka VM instances (based on the label used) for building, testing, and more.

- Support for both **Pipeline** and **Freestyle** Jenkins jobs.
- Supports both Jenkins Node/Agent Templates and Labels (under Configure Clouds) and Dynamic Labelling (in your repo's Jenkinsfile).
- The VMs become deleted after every job finishes.
- Supports automated VM Tag creation after your jobs run. This "Anka VM Template/Tag Creation" feature helps optimize subsequent builds as the dependencies are already on the VM.
- Supports both **JNLP** and **SSH** based connections to Anka VMs.
  
> In order to follow these instructions, you will need to [install the Anka CLI]({{< relref "anka-virtualization-cli/getting-started/installing-the-anka-virtualization-package.md" >}}) and an understanding of how to [start the VM]({{< relref "anka-virtualization-cli/command-line-reference.md#start" >}}) and [launch the viewer]({{< relref "anka-virtualization-cli/command-line-reference.md#view" >}}).

## Anka VM Template & Tag Preparation

In order for Jenkins remoting to communicate with your Anka VMs and turn them into agents, you'll need to prepare the VMs. The primary dependency to install inside of the VM is JRE/JAVA. However, there are other tweaks which depend on the Launch Method you'll choose when configuring our Jenkins plugin.

{{< hint info >}}
###### **Launch Methods Explained**

  - `SSH`: Jenkins will SSH into the (ssh-slaves plugin) VM once it's running, inject remoting.jar (using the [`ssh-slaves` plugin](https://github.com/jenkinsci/ssh-slaves-plugin)), and then execute `java` [remoting.jar](https://github.com/jenkinsci/remoting/) to establish the communication. It requires that your VM has port forwarding set up for port 22.

  - `JNLP`: This will start the VM and use `anka run` on the host to execute a ["startup_script"]({{< relref "anka-build-cloud/working-with-controller-and-API.md#start-vm-instances" >}}) that downloads the Jenkins remoting.jar using the Jenkins URL you configured and then establishes the communication with the inbound agent port Jenkins provides (editable under `Configure Global Security > TCP port for inbound agents`). This does not need SSH.
{{< /hint >}}

{{< hint warning >}}
The JRE/JAVA version inside of the VM **must match the version inside of Jenkins**.
{{< /hint >}}

### Dependency Installation & Configuration

1. **In the VM**, install the proper JRE/JAVA version:
    1. Within Jenkins, visit **/systemInfo** (`System Properties`) and look for `java.version`.
    2. Use the value to determine the proper JRE/JAVA version you need to download and install in your VM Template.

As an example using the Anka CLI:

```bash
sudo anka run 14.3-jre17.48.15 bash -c '
  set -exo pipefail;PATH=$PATH:/usr/local/bin:/opt/homebrew/bin;
  [[ $(arch) == arm64 ]] && export ARCH=aarch64 || export ARCH=x64;
  rm -rf zulu*;
  curl -v -L -O https://cdn.azul.com/zulu/bin/zulu17.48.15-ca-jre17.0.10-macosx_${ARCH}.tar.gz && [ $(du -s zulu17.48.15-ca-jre17.0.10-macosx_${ARCH}.tar.gz  | awk '\''{print $1}'\'') -gt 80000 ] && \
  tar -xzvf zulu17.48.15-ca-jre17.0.10-macosx_${ARCH}.tar.gz && \
  sudo mkdir -p /usr/local/bin && for file in $(ls ~/zulu17.48.15-ca-jre17.0.10-macosx_${ARCH}/bin/*); do sudo rm -f /usr/local/bin/$(echo $file | rev | cut -d/ -f1 | rev); sudo ln -s $file /usr/local/bin/$(echo $file | rev | cut -d/ -f1 | rev); done && \
  java -version && [[ ! -z $(java -version 2>&1 | grep 17.48.15) ]]'
```

#### SSH Launch Method

  2. In the VM, make sure remote login is enabled (`System Preferences > Sharing`).
  3. On the host, enable SSH [port forwarding]({{< relref "anka-virtualization-cli/command-line-reference.md#modify-nameuuid-port" >}}) for your VM Template using the Anka CLI: `sudo anka modify <VM Template name> add port --guest-port 22 ssh`. _We recommend not specifying --host-port._

#### JNLP Launch Method

Starting around Big Sur, Apple has introduced security requirements which can cause problems with `anka run` execution inside of the VM to start the JNLP process. We need to manually add `ankarund` inside of the VM's Security settings.

  2. Start your VM and run the following command **on the host's terminal**: `anka run vmtemplate osascript -e 'tell application "System Events" to keystroke "Hello World"'`. This will trigger the security warning dialog on the desktop. 
  
      ![security prompt]({{< siteurl >}}images/ci-plugins-and-integrations/jenkins/ankanetd-system-events-warning.png)

  3. Once approved, go into `System Preferences > Security & Privacy > Privacy > Accessiblity` and make sure `/Library/Application\ Support/Veertu/Anka/addons/ankarund` is added and allowed (checked) just as the image below shows.

      ![security prefs]({{< siteurl >}}images/ci-plugins-and-integrations/jenkins/ankanetd-security-and-privacy.png)

4. `sudo anka suspend <VM Template name>`
5. `sudo anka registry push <VM Template name> <Tag name>`

## Install and Configure the Anka Plugin in Jenkins

> These steps are based on Jenkins 2.289.1 and the Anka v2.6.0. Your version/UI may differ slightly.

1. Navigate to `Manage Jenkins > Manage Plugins` and click on the **Available** tab. Search in **Filter** for "Anka", then install it. _You won't see any results if it's already installed._

2. Now you can configure the Anka Cloud. Navigate to `Manage Jenkins > Manage Nodes and Clouds` and find the section called `Configure Clouds`. Inside of `Configure Clouds`, click on **Add a new Cloud** at the bottom of the page and then click on **Anka Build Cloud Plugin**.

3. You now have an **Anka Build Cloud Plugin** panel exposed to do your configuration. You can set **Cloud Name** to be anything you want. However, you'll need to set **Full Anka Build Cloud Controller URL** to the proper URL of your controller, and _**make sure you include the port**_: `http://<ip/domain>:80`. Once you're done, click **Save**.

At this point you can either setup [Static Labels]({{< relref "#creating-static-labels" >}}) or use [Dynamic Labelling]({{< relref "#using-dynamic-labelling" >}}) in your Jenkinsfile. 
- **Static Labels** are attached to **Agent/Node Templates** and can then be referenced in your pipeline or job definitions.
- **Dynamic Labels** are set up inside of the Jenkinsfile inside of a pipeline, stage, or step-level definition.

### Creating Static Labels

1. Return to `Manage Nodes and Clouds > Configure Clouds`. You should now see a **Jenkins Node/Agent Templates and Labels** section. Make sure **Show Agent/Node Templates** is checked and click on **Add**. This creates a new Template you can edit.

2. Under the Template, you want to select the proper **Anka VM Template** and **Anka VM Template's Tag** or leave the tag selection to the default value to select the latest Tag in the Registry.

3. (Optional) Set **Node/Agent Description** 

4. Select the **SSH** or **JNLP** method for connection between Jenkins and your Anka VMs.

    - SSH: You'll need to add the proper user credentials to Jenkins. If you're using the default user on the VM, use the user: `anka` and pass: `admin`.
    - JNLP: This method downloads an agent.jar and the slave-agent.jnlp file from the **Jenkins URL** you've set in your System Configuration into the VM. 
        - If the Jenkins URL doesn't resolve inside of the VM (like if it's set to http://localhost), you won't be able to use JNLP. 
        - _If you're seeing any problems post-creation of the agent inside of Jenkins, check the running VMs `/tmp/log.txt` for errors._
        - The agent.jar will be [downloaded on the VM using `curl`](https://github.com/jenkinsci/anka-build-plugin/blob/2c20ba0c22e0c4d164bf8175128064754a301908/src/main/java/com/veertu/plugin/anka/JnlpCommandBuilder.java#L14) and if the VM does not have your root CA available to validate the Jenkins HTTPS, you will need the VM to trust it with `sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ca-crt.pem`.

5. (Optional) Set **Maximum Allowed Nodes/Agents** if you need to limit the amount of agents that any jobs are allowed to start.

6. (Optional) Set **Allowed Executors** if you want to allow a single agent to be used for multiple jobs.

7. (Optional) Set **Anka VM Workspace Path** if you'd like for the job to work within a specific path inside of the VM.

8. (Optional) Set any **Environment Variables** necessary for your jobs

9. There are several other configs under **Advanced** that may interest you. These include the **Node Group**, **Priority**, and timeouts for Jenkins to communication with the starting Anka VM/agent.

### Using Dynamic Labelling

> Dynamic Labelling requires two plugins: **GitHub Authentication plugin** and **Pipeline**.

This section describes the steps to create dynamic labels inside of your Jenkinsfile using the `createDynamicAnkaNode` function.

- The `createDynamicAnkaNode` function starts a VM and returns a unique label for it.
- The returned label can be interpolated in your pipeline, stage, or step-level definitions. ([Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/simple-example/Jenkinsfile))
- `createDynamicAnkaNode` has all the configuration options that the UI configured Static Slave Template has.
- This allows you to have only a minimal Anka Cloud configuration and define Anka Nodes on the fly.
   
> Jenkins has a Pipeline Snippet Generator, which helps you craft your `createDynamicAnkaNode` definition. You can find it in your Jenkins instance at `/pipeline-syntax`.

#### `createDynamicAnkaNode` Parameters

> You can obtain the "masterVmId" (VM UUID) using: `sudo anka show <Template name> uuid`

**Name** | **Type** | **Default Value** |  **Description** |  **Required**
--- | --- | --- | --- | ---
cloudName | String | -  | Cloud to target, by name | -
masterVmId | String | -  | UUID of the VM Template | Yes
tag | String | -  | VM Template Tag name | -
remoteFS | String | `Users/anka` | Remote node workspace | -
launchMethod | String | `jnlp` | Node launch method: ‘ssh’ or ‘jnlp’ | -
credentialsId | String | -  | Jenkins credential ID for ssh launch method | -
extraArgs | String | -  | String to be appended to JNLP command | -
javaArgs | String | -  | String to append to JNLP java args | -
jnlpJenkinsOverrideUrl | String | -  | Override the Jenkins server url set in the Jenkins configuration | -
jnlpTunnel | String | -  | JNLP tunnel to use for node launcher | -
keepAliveOnError | boolean | `false` | Keep the VM instance alive after a failed build (**FREESTYLE ONLY; will not work with Jenkinsfile/pipeline jobs**) | -
timeout | int | `1200` | Timeout for starting the instance (in seconds) | -
environments | List of tuples | -  | List of environment variables to add for the build: `[[name: 'FOO', value: 'BAR'], [name: 'OR', value: 'IS']]` | -
nameTemplate | string | -  | Label to use in VM instance names (There are several variables available for interpolation: $template_name, $template_id, $tag, or $ts) | -
priority | int | 1000  | Override the default priority (lower is more urgent). **Available only in Enterprise and Enterprise Plus Tiers** | -
saveImage | boolean | `false` | Save the VM as a Tag before terminating it | -
suspend | boolean | `false` | When saving the Tag, suspend the VM before the push | -
TemplateId | string | -  | When saving the Tag, push onto a specific Template UUID | -
pushTag | string | -  | When saving the Tag, set the Tag name (A timestamp will be appended) | -
deleteLatest | boolean | `false` | When saving the Tag, delete the latest Tag for the Template out of the registry before pushing (Dangerous: only use if the Template isn't holding other project tags) | -
TemplateDescription | string | -  | When saving the Tag, set a description for the new Tag | -
group | string | -  | Group ID to start the instance in (_Available only in Enterprise and Enterprise Plus Tiers_) | -
numberOfExecutors | int | `1` | Number of Jenkins executors to run on the Node (we recommend 1) | -
description | string | -  | On creation of the instance, set the description | -
labelString | string | -  | Override the returned label (not recommended) | -
dontAppendTimestamp | boolean | `false`  | Do not append timestamp to the tag being pushed | -
vcpu | int | -  | Sets the CPUs for the VM before starting it (requires that the VM is stopped) | -
vcpu | int | -  | Sets the RAM for the VM before starting it (requires that the VM is stopped AND that the value is in MBs) | -

#### Dynamic Jenkinsfile Example

```javascript
def NODE_LABEL = createDynamicAnkaNode(
  masterVmId: 'e1173284-39df-458c-b161-a54123409280'
)
pipeline {
  agent {
    label "${NODE_LABEL}"
  }
  stages {
    stage("hello") {
      steps {
        sh "echo hello"
      }
    }
  }
}
```

- In this example, the pipeline uses the "NODE_LABEL" variable defined at the beginning of the file.
- The function call starts an instance with the Template UUID 'e1173284-39df-458c-b161-a54123409280' and returns a unique label, preventing any other external step or pipeline from using it. Any unspecified parameters have their default values, as indicated in the parameter list above.
- The sh step waits until the instance is up and the node is connected to Jenkins before it runs.

##### Examples

- [Simple Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/simple-example/Jenkinsfile)
- [Scripted Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/scripted-example/Jenkinsfile)
- [Scripted (no def) Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/scripted-no-def-example/Jenkinsfile)
- [Nested Cache Builder Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/nested-cache-builder-example/Jenkinsfile)

#### Log Investigation

Investigation of problems which arise in Jenkins + Anka can be a bit tough to get started with.

Let's take this Jenkinsfile as an example:

```javascript
pipeline {
  agent {
   label createDynamicAnkaNode(
      masterVmId: 'c0847bc9-5d2d-4dbc-ba6a-240f7ff08032',
      tag: 'v1',
      nameTemplate: 'simple-example'
    )
  }
   stages {
     stage("hello") {
       steps {
         sh "echo hello"
       }
     }
  }
}
```

Once the job kicks off in Jenkins, you'll see the following Console Log:

```log
[Pipeline] Start of Pipeline
[Pipeline] createDynamicAnkaNode
[Pipeline] node
Still waiting to schedule task
‘simple-example_wi00u’ is offline
Running on simple-example_wi00u in /Users/anka/workspace/testing-dynamic-labelling_Simple
```

The `simple-example_wi00u` is the name of our Jenkins agent (formerly called "slave") that will be attached to the resulting Anka VM. At this point the Anka Build plugin in Jenkins, through `createDynamicAnkaNode`, will make a VM request through the Controller API. While the Console Log for the Jenkins Job is not going to provide us with much else, the **Jenkins System Logs** will:

{{< hint warning >}}
There is currently no way to obtain information about the starting VM from the logs BEFORE it has started and been connected to. This means if the VM fails to start on a node, while it will be retried on another node, you cannot easily see that it's delayed in starting because of the failure. It's best to monitor the controller for failures and troubleshoot them independently of Jenkins.
{{< /hint >}}

```log
May 02, 2022 4:48:12 PM INFO com.veertu.plugin.anka.AnkaMgmtCloud InternalLog
Node simple-example_wi00u instance 71a8cb4b-875b-49de-59cf-83b1b65304e1, connected
```

The important part of the Jenkins System Log is the `instance 71a8cb4b-875b-49de-59cf-83b1b65304e1`. With this ID, we can see things in the Controller logs like the node that tried to start it, etc.

```bash
Veertu:~ root# grep 71a8cb4b-875b-49de-59cf-83b1b65304e1 /Library/Logs/Veertu/AnkaController/anka-controller.INFO
. . .
I0502 12:47:38.546497   56377 controller.go:329] StartVm: reqID 4d6fd6b2-ba34-4c6b-6d02-80b2ce391be6, instance 71a8cb4b-875b-49de-59cf-83b1b65304e1
. . .
I0502 12:48:03.628071   56377 controller.go:627] Processing StartVMResponse: request: 4d6fd6b2-ba34-4c6b-6d02-80b2ce391be6, instance: 71a8cb4b-875b-49de-59cf-83b1b65304e1, node id: 3c101836-9540-4733-9482-604d0c5fbe30, status: 0, timestamp: 1651510083
. . .
```

The `node id: 3c101836-9540-4733-9482-604d0c5fbe30` above is what we can use to determine which Node this failed on and where we need to go for further log investigation.

---

## Using Anka VM Template/Tag Creation (Optional)

Anka VM Template/Tag Creation is useful when you need to automate the preparation of a VM with dependencies. You can setup a separate Jenkins job/pipeline to execute preparation scripts inside of a VM and then push to the registry with a specific tag. Other jobs can then target the latest tag and immediately start using the new VM template/tag.

> The plugin will push to the Registry for any buildStatus except for **FAILURE**, **ABORTED**, or **UNSTABLE**.
> You can wrap your step-level commands in `catchError(buildResult: 'FAILURE', stageResult: 'FAILURE')` or use the Build **Execute shell** advanced **Exit code to set build unstable** setting to prevent the VM from pushing.

### Configuring for Static Labels

You can find the **Anka VM Template/Tag Creation** in the **Jenkins Node/Agent Templates and Labels** section under `Manage Jenkins > Manage Nodes and Clouds > Configure Clouds`.

#### Using the Post Build Step with Static Slave Templates

![image12]({{< siteurl >}}images/anka-build/controller01.png)

**Fail build** : Will set the build as failed if the `image save request` failed (or timed out). If this is not selected, the Post Build Step does nothing.

**Timeout** : Sets the minutes to wait for the Tag push to Registry to cowmplete (this needs to be determined manually or with trial and error).

#### Using a Jenkinsfile with the Static Slave Template

Pipelines can have multiple agents running in one build (also in parallel). Therefore, the plugin relies on the buildResult to tell if it needs to execute the "save image request". You have two options to prevent pushing the new Tag prematurely:

1. Wrap your step-level commands in a catchError ([from our Nested Cache Builder Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/nested-cache-builder-example/Jenkinsfile)):
    ```javascript
    stage("run-on-NESTED_LABEL-vm") {
      agent { label "${NESTED_LABEL}" }
      steps {
        // If buildResults == 'FAILURE', Anka will not push the NESTED_LABEL VM. Example:
        catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
          sh 'uname -r;'
        }
      }
    }
    ```
2. Enable **Wait For Build Finish**

### Configuring Template/Tag Creation with Dynamic Labelling

Please review our [Nested Cache Builder Example](https://github.com/veertuinc/jenkins-dynamic-label-example/blob/nested-cache-builder-example/Jenkinsfile).

- The **AGENT_LABEL** VM instance is created to handle the first stage.
- The second stage creates a second VM instance that is configured to push to the Registry once the buildResult is a successful status.
  
> If buildResult receives any of the failed statuses (**FAILURE**, **ABORTED**, or **UNSTABLE**), the **ankaGetSaveImageResult** outputs `Checking save image status… Done!` in the Jenkins job logs/console since there is no push operation to wait on.

#### Using the Post Build Step with Dynamic Labelling

Create an `ankaGetSaveImageResult` Pipeline step:

```javascript
stage("check-generated-tag-from-nested-vm") {
  steps {
    script {
      def getPushResult = ankaGetSaveImageResult( shouldFail: true, timeoutMinutes: 120 )
      echo "ankaGetSaveImageResult Returned: $getPushResult"
    }
  }
}
```

If you're creating multiple cache Tags in a single pipeline, The `ankaGetSaveImageResult` function returns true if all previous image save calls for this particular build have executed.

**Name** | **Type** | **Default Value** |  **Decription** |  **Required**
--- | --- | --- | --- | ---
shouldFail | boolean | true | Fails the job if one of the image save requests has failed or timed out | 
timeoutMinutes | int | 120 | Stops waiting for the result of the Tag -> Registry push after x minutes. |

> Remember, `ankaGetSaveImageResult` returns true immediately if nothing pushes to the Registry in a failed build.

## Notes on using the Template/Tag Creation

- How often should I run this? : The answer depends on the VM size after you prepare it and also the density of your builds.
- **The template/tag creation should have a job or pipeline of its own.** Creation after every "regular" build might not make sense as the time that it takes to download code or artifacts is usually the same or shorter than the time it takes to push the Tag to the Registry.
- You should run it once and check how much time the operation takes. The push can be a few gigabytes and might take some time on slower networks.
- Only one Tag can push at a time. You can execute multiple creations for the same Template, but those requests end up executing serially. 
- If your build/preparation is fast and resulting templates/tags small, you can consider running creation a few times a day or even based on commits. If your creation is slow, consider running one per day (you can schedule it for a time that Jenkins is not busy).

---

## Finding Job Name and ID for a running Instance

Starting in 2.9.0, users can add a Custom Column in their Controller UI on the Instances page to see the Jenkins job name and ID for a VM.

![job name and id]({{< siteurl >}}images/ci-plugins-and-integrations/jenkins/jenkins-job-name-and-id-instances-page.png)

---

## Release Notes

### 2.12.0 - [February 25th, 2025](https://github.com/jenkinsci/anka-build-plugin/pull/30)

{{< hint warning >}}
MINIMUM REQUIRED JENKINS VERSION: `2.426.3`
{{< /hint >}}

- **Improvement:** Cloud UI redesign.
- **Improvement:** Use jenkins.baseline to reduce bom update mistakes https://github.com/jenkinsci/anka-build-plugin/pull/29
- **New Feature:** Support for UAK/TAP authentication. Select the `Secret text` credential type when creating. Make sure the ID of the credential matches that of the UAK in your Controller. The text will be the `key string` that was created from the PEM you downloaded when you created the UAK (to generate it from the PEM file, use `cat uak-key.pem | sed '1,1d' | sed '$d' | tr -d '\n'`).
- **Bug Fix:** Errored instances caused Jenkins to grind to a halt.

### 2.11.0 - April 10th, 2024

{{< hint warning >}}
MINIMUM REQUIRED JENKINS VERSION: `2.387.3`
{{< /hint >}}

- **Improvement:** Way better logging in various parts of the plugin.
- **Improvement:** When specifying a non-existent key for `createDynamicAnkaNode`, you will now see an error in the job's Console Output.
- **Improvement:** Support for new modern PKCS8 (as well as the older PKCS1), RSA, and EC certs.
- **New Feature:** Ability to target a specific Cloud by setting `cloudName` in `createDynamicAnkaNode`.
- **New Feature:** Ability to use Bridge mode VMs by waiting for the VM to obtain an IP from DHCP (you can configure this from Advanced in the Clouds > Anka Cloud you've created).
- **Bug Fix:** Certificate Authentication was forcing all Clouds to use the same certificate. They are now separate and "None" is now a dropdown option to select if no Cert is needed.
- **Bug Fix:** Skip TLS Verification was not working and required a CA Root.


### 2.10.0 - Oct 30th, 2023

- **Bug Fix:** UI elements were distorted and larger than necessary.
- **New Feature:** Ability to set vcpu and vram for stopped VMs prior to starting.
- **Improvement:** Various security patches.

### 2.9.0 - Jul 21st, 2022

- **Bug Fix:** Misleading title and description for "name template" in Jenkins Plugin template config
- **Improvement:** Unique runOnceCloud symbol to not collides with other plugins
- **Improvement:** Improved logging for http errors between plugin and controller
- **Improvement:** KeepAliveOnError feature in plugin UI now has an indication it's for FreeStyle jobs only

### 2.8.0 - Jun 13th, 2022

- Support for Controller communication over the configured Jenkins Proxy.

---

### 2.7.0 - Jan 10th, 2022

- Security patches

---

### 2.6.1 - Dec 23rd, 2021

- **Bug Fix:** When running on Jenkins 2.319, "null" was being passed when tag was set to latest (empty)


{{< hint warning >}}
For either 2.7.0 or 2.6.1, please check over your Anka Build Plugin configuration and agent/label templates and ensure things are correct. **Even if correct, please issue a Save of the configuration post-upgrade.**
{{< /hint >}}

---

### 2.6.0 - June 29th, 2021
- Improvement: New UI design, field names, and descriptions
- Bug Fix: Jenkins agent templates are deleted when the Anka Build Cloud URL changes

---

### 2.5.0 - Mar 30th, 2021
- Support for Jenkins versions `2.277.1` and above (new UI changes)

> Breaking change: Versions < 2.260 of Jenkins are not supported

---

### 2.4.0 - Feb 18th, 2021
- New Feature: [You can now set the Launch timeout values to handle network/resource conditions that delay VMs initializing their networking]({{< relref "whats-new/anka-legacy.md#set-various-vm-launch-timeout-values" >}})

---

### 2.3.0 - Dec 1st, 2020
- New Feature: Disable appending timestamp to Cache Builder/tags

---

### 2.2.1 - Nov 11, 2020
- Bug Fix: Dynamic Anka Node pipeline step causes job to hang

---

### 2.2.0 - August 31, 2020
- Bug Fix: vmPollTime definitions in Configuration as Code aren't working due to typo

    > **If updating from 1.X.X: This version (2.X) requires that you back up your Static Slave Templates (config.xml) and add them again.**

---

### 2.1.2 - July 8, 2020

> **If updating from 1.X.X: This version (2.X) requires that you back up your Static Slave Templates (config.xml) and add them again.**

- Bug Fix: createDynamicAnkaNode remoteFS and launchMethod are being ignored
- Bug Fix: HTTPS without certificate authentication enabled doesn't work

---

### 2.1.0 - June 22, 2020

> **If updating from 1.X.X: This version (2.X) requires that you back up your Static Slave Templates (config.xml) and add them again.**

- Various stability / performance improvements
- New Feature: Node/slave name and jenkins url is passed to the controller to display within the instances page
- New Feature: A warning will display when users upgrade within the plugin center
- New Feature: Ability to set an instance cap per Static Slave Template or per Anka Cloud
- Bug Fix: VMs were being left in started stage after job completed/aborted in Jenkins
- Bug Fix: When cache building, "Checking save image status" would immediately return success and the Job would complete even though the cache tag was still being pushed.
- New Feature: Users can now add a Custom Column named `jenkins-job-id` to see the Jenkins job name and number.


---

### 1.23.0 - Mar 23, 2020
- Bug Fix: Controller IP is inaccessible from the Jenkins and Jenkins page is stuck on “loading” forever
- Issues in amazon-ecs-plugin version 1.26 cause problems with Jenkins-Anka Plugin. In order to fix this, downgrade to amazon-ecs-plugin version 1.22 or disable amazon-ecs-plugin. Check https://github.com/jenkinsci/amazon-ecs-plugin/issues/158 for more details.

---

### 1.22.4 - Mar 08, 2020
- Bug Fix: leaves zombie VMs after Jenkins upgrade to 2.204
- Bug Fix: Jenkins leaves zombie VMs when restarting
- Bug Fix: Jenkins leaves zombie VMs when job has error on JNLP node
- Bug Fix: Jenkins Job changing Jenkins Master node labels
- Bug Fix: Jenkins JNLP Job starts more instances than it should
- Bug Fix: Jenkins config hangs of controller IP is inaccessible

***Note*** Currently issues in amazon-ecs-plugin version 1.26 cause problems with Jenkins-Anka Plugin. In order to fix this, downgrade to amazon-ecs-plugin version 1.22 or disable amazon-ecs-plugin. Check https://github.com/jenkinsci/amazon-ecs-plugin/issues/158 for more details.

---

### 1.22.3 - Jan 27, 2020
- Bug Fix: Jenkins takes nodes offline before restart
- Bug Fix: Jenkins logs Anka related messages when other agents (non anka) disconnects
- Bug Fix: Jenkins leaves zombie VMs when restarting

---

### 1.22.2 - Dec 08, 2019#
- Bug Fix: Jenkins gets an exception while trying to start a new slave with OR operator

---

### 1.22.1 - Nov 15, 2019
- Bug Fix: ankaGetSaveImageResult does not work for jobs in folders
- Bug Fix: Saving new cloud configuration would crash jenkins if controller does not support save image

---

### 1.22.0 - Nov 05, 2019
- New Feature: Adjust Jenkins Anka build plugin to "Configuration as code" plugin
- New Feature: Implement dynamic slaves in jenkins anka build plugin
- Bug Fix: Cant access jenkins configuration page if controller is offline

---

### 1.21.0 - Oct 18, 2019
- New Feature: Add a "post build step" to check "save image request" status
- Bug Fix: cache builder - JNLP, Suspend setting doesn't work

---

### 1.19.1
- New Feature: handle reference to non-existent tag in registry from gracefully
- Bug Fix: Support ssh slaves 1.30.0 in jenkins Plugin
- Bug Fix: Jenkins pipeline job doesn't terminate slaves after finishing
- Bug Fix: Plugin not taking slave name field for JNLP

---