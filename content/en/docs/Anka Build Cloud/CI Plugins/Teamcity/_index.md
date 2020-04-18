

---
date: 2019-07-03T22:24:47-05:00
title: "Using Teamcity and Anka Build Cloud"
linkTitle: "Teamcity"
weight: 6
description: >
  Instructions on how to use Teamcity with Anka Build Cloud
---


The Anka TeamCity plugin provides a quick way to integrate Anka Build Cloud with TeamCity for iOS/macOS CI workflows. This enables TeamCity jobs to dynamically provision specific macOS VM instances on Anka Build Cloud for job execution. The VMs are deleted after every successful job execution. New job request starts a brand new VM instance on the Anka Build Cloud.  


### Preparing the VM Template
1. Install openJDK 8 in your macOS VM Template.
2. Create Teamcity User(required for ssh based connectivity from Teamcity to Anka VMs). Make sure remote login is enabled for this user. You can also use default `anka` user.
3. Enable port forwarding (required for ssh based connectivity from Teamcity to Anka VMs).
4. Download TeamCity agent executable in the Vm under the newly created TeamCity user.
5. Go to `BuildAgent/conf/buildagent.properties` under Teamcity user in the VM and check all the settings. Make sure that server url pointing to your Teamcity server IP as `serverUrl=http://xx.xxx.xx.xxx` is correct.
6. Start the Teamcity agent service inside the VM with `sh BuildAgent/bin/mac.launchd.sh load`.
7. Connect to this VM from Teamcity, wait for few minutes for Teamcity to connect and authorize the VM agent instance. It will show up under the connected tab in Agents in TeamCity.
5. Suspend the VM with `anka suspend <template>`.
6. Push it to anka registry.

### Configure the TeamCity Plugin
1. Install the Anka Build Cloud Plugin - Browse to `Administration > Plugins List`. Upload the anka-build-tc-XX.zip.   

2. Create Cloud profile for the project and select `Anka Build Cloud` for __Cloud Type__. 

3. Edit the Cloud profile to complete configuration.

4. For __Additional terminate conditions__, select `after first build`.

5. For __Controller URL__, enter Anka Build Cloud controller IP and port. Default port is 80. 

6. __Image Name__ will show all VM templates from the Registry. Select the one you want to use for build/test for this project.
  
7. __Image Tag__ will show all tags for the selected VM template in the __Image Name__ field. Select the one you want to use for your build/test. Leave it to latest to always select the latest one.  

8. For __Group (optional)__, if you want VMs for this definition provisioned on a specific group defined in your Anka Build Cloud (Available only in Enterprise and Enterprise Plus Tiers). 

9. __SSH User__ and __SSH Password__ are the VM Teamcity user ssh credentials. 

10. __Agent Path Select__ is the path to TeamCity agent location in the VM.
   
11. Select a __Node Group__ if you want VMs for this definition provisioned on a specific group defined in your Anka Build Cloud (Available only in Enterprise and Enterprise Plus Tiers).  

12. Enter __Priority__ for Vm provisioning (Available only in Enterprise and Enterprise Plus tiers).  

13. Save the settings.  
