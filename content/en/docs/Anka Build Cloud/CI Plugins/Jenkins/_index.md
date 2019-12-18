---
date: 2019-07-03T22:24:47-05:00
title: "Using Jenkins and Anka Build Cloud"
linkTitle: "Jenkins"
weight: 5
description: >
  Instructions on how to use Jenkins with Anka Build Cloud
---

The Anka Jenkins plugin provides a quick way to integrate Anka Build Cloud with Jenkins for iOS/macOS CI workflows. This enables Jenkins jobs to dynamically provision specific macOS VM instances(based on label used) on Anka Build Cloud for job execution. The VMs are deleted after every successful job execution. New job request starts a brand new VM instance on the Anka Build Cloud.  

Anka Jenkins plugin supports both **Pipeline** and **Freestyle** Jenkins jobs.

***NOTE*** : Starting from version 1.20 of Anka jenkins Plugin, the Slave template builder plugin features are integrated in Anka Jenkins plugin. It's possible to uninstall Slave template builder plugin, install the Anka Jenkins plugin version 1.20 and configure cache builder settings similar to Slave Template builder configuration.


### Preparing the VM Template

1. Install openJDK 8 in your macOS VM Template.
2. Create Jenkins User(required for ssh based connectivity from Jenkins to Anka VMs). Make sure remote login is enabled for this user. You can also use default `anka` user.
3. Enable port forwarding (required for ssh based connectivity from Jenkins to Anka VMs).
4. Suspend the VM.
5. Push it to anka registry.

#### SSH or JNLP based Connection from Jenkins to Anka VMs
Anka Build Jenkins plugin supports both JNLP and SSH based connections to Anka VMs.  

**JNLP** -  For JNLP, configure for JNLP in the Anka Build plugin in Jenkins.

![image5](/images/using-jenkins/anka-cloud5.png)

**SSH**  

1. Create a dedicated (Jenkins) user and enable remote login for this user in the VM template. Make sure to enable auto login for the administrative user of the VM as well.
2. Configure port forwarding


### Configure the Anka Cloud Plugin in Jenkins

1. Install the Anka Build Cloud Plugin - Browse to `Manage Jenkins > Manage Plugins`.  

2. Click on `Available`. Search for Anka and you can install it from jenkins Plugin center or click on the `Advanced` tab. Use the instructions under the __Upload Plugin__ section to upload the `anka-ci.hpi` plugin file to your Jenkins master server.  

3. Go to `Manage Jenkins > Configure System` and find the section called __Cloud__. Enter a name for your Anka Cloud in the Anka Build Cloud field, as well as an IP address and port number for the controller in the Build Controller URL field(http://xx.xxx.xxx.xxx:portno). Default port is 80.

4. Click __Show Templates__ and configure Jenkins slave templates for your jobs.  

5. Select the VM from the __Templates__. This will show the list of all VMs from the registry.  

6. Select the tag/version from __Template Version Tag__ dropdown. Leave it to Latest if you want to always access the latest tag/version of the VM.  

7. Leave __# of Executors__ to 1.  

8. Enter the Jenkins user home path in __Remote FS Root__.  

9. Enter a value for __Labels__ that you will refer to in your jobs.  

10. Select SSH or JNLP method for connection between jenkins and Anka VMs.  

11. Enter value for __Slave name template__. Vms provisioned for this slave template definition will have this value.  

12. You can also pass Environment Variables.  

13. Select a __Node Group__ if you want Vms for this definition provisioned on a specific group definied in your Anka Build Cloud (Avaialble only in Enterprise and Enterprise Plus Tiers).  

14. Enter __Priority__ for Vm provisioning (Available only in Enterprise and Enterprise Plus tiers).  

15. Apply and Save the settings.  

**Note** You can save multiple slave VM template profiles with a unique label name corresponding to different VM template types/tags. Then, your jobs can refer to these labels.

#### Configure the Cache Builder Section(Optional)

This section integrates the Jenkins Slave template Builder Plugin functions into the Anka jenkins plugin, to eliminate the need to install two plugins.

![image11](/images/using-controller/image11.png)

Configure this section if you want to pre-populate the VM templates with build caches through jenkins job, save them back to the registry and use them for your mainline jobs.

**Target Template** : This is the template to push the cache populated VM back to. Most of the times, it will be the same VM template you used to build caches in.

**Tag** : Specify the tag you want to push the cache populated VM template with. If left empty, it will take the jenkinsjobname and append current date/time to it.

**Description** : Decription for the VM being pushed.

**Suspend** : Select, if you want to push the Vm in suspended or stopped state

**Delete Latest Tag** : Delete the latest tag from the registry before pushing this tag. This will enable optimization of registry disk space, of the cache builder job is run frequently.

**Wait For Build Finish** : When using pipeline, a step or stage might finish before the build has a result. The plugin uses the build result as the condition for saving the image. In case there is no result yet, the plugin will push the template. You can use ‘currentBuild.result’ in your build step to mark the job as failed. Defaults to false.
***Note*** - Do not use Wait for Build Finish option in combination with ankaGetSaveImageResult, as it can result in a deadlock.

**Pipeline Usage for cache builder section**
Pipelines can have multiple agents running in one build (also in parallel). The plugin relies on the build result in order to tell if it need to execute the “save image request” or not (the request will only be sent if the build succeeded).

Pipeline stages can be executed and done before the build has a result, so we can use one of two solutions.

**Mark the build from the pipeline**
	As explained here: https://support.cloudbees.com/hc/en-us/articles/218554077-How-to-set-current-build-result-in-Pipeline
	It is possible to set ‘currentBuild.result’ from within the pipeline itself. 
	With this option if the build will be marked as “FAILED” “ABORTED” or “UNSTABLE”
	The save image request will not be issued

**Wait for the build to finish**
	Check “wait for build finish” in the slave template cache builder section. 
	When this checkbox is checked the slave will be kept alive until the build attached to it gets a result.  

**Post Build Step to check status of Anka Image Save action**

![image12](/images/anka-build/controller01.png)

Fail build: Will set the build as failed if the `image save request` failed (or timed out). If this is not selected, the post build step will do nothing.

Timeout: The timeout is set in minutes and should be long enough to wait for the image push to registry be complete. If the value is very small, it will generate a false timeout error.

**Using Post Build Step to check status of Anka Image Save action in Pipeline**

Use the `ankaGetSaveImageResult` Pipeline step. 

![image13](/images/anka-build/controller02.png)

`ankaGetSaveImageResult` will return true if all previous image save calls for this particular build have been successfully executed.

shouldFail: Type - Boolean value. Fails the job if one of the image save requests has failed or timed out (default: true)

timeoutMinutes: Type - Integer in minutes. Stops waiting for the result of the template update after x minutes. (default: 120)

`ankaGetSaveImageResult` will return true immediately if no calls have been made.

***NOTE*** If “Wait For Build Finish” is marked for the label’s Cache Builder configuration, this step will identify no image saving requests and thus will return true immediately.

##### Using Jenkins Plugin Cache Builder With Pipeline

***Overview***

1. A build runs on a VM provisioned by the plugin.

2. When the build is finished (in case of success) the plugin will issue a “save image” request to the controller instead of terminating the VM.

3. The VM will then be pushed to the registry defined in the controller configuraton.

4. The build will get the result of the push using ankaGetSaveImageResult.

Follow the earlier steps to configure the plugin and the cache builder section.

***Example Pipeline***

Scripted Pipeline example

```
def label = "CacheVM"
 
node(label){
    stage('Build Cache') { 
        try {
            // scm pull… 
            sh 'sleep 20' // do whatever 
            // error("simulated error!")
        } catch (Exception e) {
            currentBuild.result = 'FAILURE'  //  fail the build on any error
        }
        
    }
}
 
ankaGetSaveImageResult shouldFail: true, timeoutMinutes: 120
```
When you run this example you should see the instance in “pushing” state as shown below.
![image14](/images/anka-build/image14.png)

When the build is finished the console output will be as follows.
![image15](/images/anka-build/image15.png)

Uncomment the ‘error’ row in the earlier `Scripted pipeline example` to simulate error in pipeline. Now, the build will fail and the template is not pushed.
![image16](/images/anka-build/image16.png)

***Note*** ‘ankaGetSaveImageResult’ will still output “Checking save image status… Done!”, since there is no push operation to wait for.


#### General observations about using Cache Builder

1. How often should I cache? - The answer of course depends on the size of your data, and the density of your builds.

2. You should run your cache build once, check how much time the operation takes. The push can be a few gigabytes big (at least 2 GB when using suspend) and might take some time on slower networks. 

3. Note that only one template tag can be pushed at a time. You can execute multiple cache builds for the same template but those requests will end up executed in a serial manner. 

4. If your cache build is fast - you should consider running cache builds a few times a day, or based on commits.

5. If your cache build is slow - Consider running one cache build per day (you can schedule it for a time that jenkins is not very busy).

6. The cache build should have a job or pipeline of it’s own. Caching after every “regular” build might not make sense as the time that it takes to download code or artifacts is usually the same or shorter than the time it takes to push a registry to the vm. 

#### Creating Dynamic Label
This section describes the steps to create dynamic label for an AnkaNode. Use `createDynamicAnkaNode` function.

> The pipeline function/step `createDynamicAnkaNode` starts a vm and returns a label.
> The returned label can be used as an agent in the node agent part.
> Dynamic Anka nodes has all the configuration options that a static Anka slave template has, with slight changes. The configuration options are passed to the function as parameters.
> This allows you to have only a minimal anka cloud configuration and define Anka nodes on the fly.

> You can check out the examples on this page - https://github.com/veertuinc/jenkins-dynamic-label-example.
> For a helper form, search for `createDynamicAnkaNode` on the path http://your.jenkins.server/pipeline-syntax on your jenkins server.

*** Parameters for `CreateDynamicAnkaNode` Function ***

**Name** | **Type** | **Default Value** |  **Decription** |  **Required**
--- | --- | --- | --- | ---
masterVmId | String |   | the uuid of the VM template to run | Yes
tag | String |   | VM template tag | 
remoteFS | String | Users/anka | Remote node workspace | 
launchMethod | String | jnlp | Node launch method ‘ssh’ or ‘jnlp’ | 
credentialsId | String |   | Id of jenkins credentials object for ssh | 
extraArgs | String |   | String to be appended to jnlp command | 
javaArgs | String |   | String to append to jnlp java args | 
jnlpJenkinsOverrideUrl | String |   | Override the jenkins server url. Jenkins url is taken from jenkins configuration | 
jnlpTunnel | String |   | Jnlp tunnel to use for node launcher | 
keepAliveOnError | boolean | false | Keep the instance alive on case of build error | 
timeout | int | 1200 | Timeout for starting the instance (in seconds) | 
environments | List of tuples |   | List of environment variables to add for the build. environments: [[name: 'FOO', value: 'BAR'], [name: 'OR', value: 'IS']] | 
nameTemplate | string |   | Template to use for vm and node name. You can use $template_name, $template_id, $ts | 
priority | int |   | Override the default priority (lower is more urgent) | 
saveImage | boolean | false | Save the vm as an image instead of terminating it | 
suspend | boolean | false | When saving image suspend the vm before the push | 
templateId | string |   | When saving image, push the vm to this template | 
pushTag | string |   | When saving image, a string to use as tag. A timestamp will be appended | 
deleteLatest | boolean | false | When saving image, delete the latest version before pushing | 
templateDescription | string |   | When saving image, use the description for the new tag | 
group | string |   | Group id to start the instance to (only available in enterprise) | 
numberOfExecutors | int | 1 | Number of jenkins executors to run on this node (recommended 1) | 
description | string |   | Description for this template | 
labelString | string |   | Override the returned label (not recommended) | 

***Note - Example Code***

```
def LABEL = createDynamicAnkaNode(masterVmId: 'e1173284-39df-458c-b161-a54123409280')
 
pipeline {
 agent {
      label "${LABEL}"
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

> In this example, the pipeline uses the “LABEL” variable just as a regular string label would be used.
> The function call `createDynamicAnkaNode(masterVmId: 'e1173284-39df-458c-b161-a54123409280')` will start an instance with the template id 'e1173284-39df-458c-b161-a54123409280', using the jnlp method. The remaining node properties will have their default values as mentioned in the table above.
> The command will block until the instance is up and the node is connected to Jenkins. The function will then return a generated label. The generated label will assure that no other job would use that specific node.
 

