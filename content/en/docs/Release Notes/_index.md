---
date: 2018-03-06
title: "Release Notes"
linkTitle: "Release Notes"
weight: 100
description: >
  Detailed Release Notes 
---

## Anka (supported on all mac hardware including 2018 Mac Mini)
### Latest Version - Anka Version 2.2.0 and Controller & Registry 1.5.4
- Anka Package - Version 2.2.0 - Jan 10, 2020
- Anka Controller & Registry combined Linux package - Version 1.5.4 - Jan 15, 2020
- AnkaController & Registry Mac package - - Version 1.5.4 - Jan 15, 2020
- Anka Jenkins plug-in - version 1.22.3 - Jan 27, 2020
- Anka Teamcity plug-in - version 1.7.0 - Nov 05, 2019
- Anka GitLabCI Integration - Updated Dec 08, 2019
- Anka Packer Plugin Integration - Updated Dec 08, 2019
- Anka Jenkins Slave template Builder plug-in - (Maintained only until version 1.5). Combined with Anka Jenkins plug-in version 1.20.

### Change log Jenkins Plugin version 1.22.2 - Jan 27, 2020
- Bug Fix : Jenkins takes nodes offline before restart
- Bug Fix : Jenkins logs Anka related messages when other agents (non anka) disconnects
- Bug Fix : Jenkins leaves zombie VMs when restarting

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.5.4 - Jan 15, 2020#
- Bug Fix : Version 1.5.3 was overwriting /usr/local/bin/anka-controllerd file

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.5.4 - Jan 15, 2020#
Other : Version number change to 1.5.4 to make it match with the mac package

### Change Log Anka 2.2.0 change - Jan 10, 2020
- Bug Fix : vmnet fails to create interface
- Bug Fix : Network error (discovered while stress testing)
- Bug Fix : anka clone doesn’t randomize MAC addresses
- Bug Fix : Samsung Galaxy S9 claim leads to host panic
- Bug Fix : anka registry pull doesn’t take in scope cache size when determining free space
- Bug Fix : nvram values don’t get saved
- Bug Fix : Not possible to start more than one VM without GUI session
- Bug Fix : vmnet fails to create more than few instances of virtual interfaces without GUI session
- Bug Fix : Exceeding host based license message is missleading
- Bug Fix : mDNS requests looks fail from inside VM
- Bug Fix : Shared folders work bad for GUI usecases
- Bug Fix : vmnet doesn’t restore network connectivity after host sleep
- Bug Fix : Android devices aren’t recognized in Android Studio
- Bug Fix : The source command doesn’t pickup changes in PATH environment. (requires anka start -u for the VMs)
- Bug Fix : Anka.pkg downgrades AnkaAgent.pkg
- Bug Fix : Bad product type for Anka Build Lite
- Bug Fix : Anka run --wait-network doesn’t seem to work
- New feature : Install AnkaView into /Applications
- New feature : Allow to modify networking type of suspended VM
- New feature : Allow to create VM with manual install process
- New feature : Pass thru Anka license type from host to nested vm
- New feature : Allow to start VMs via AnkaView interface
- New feature : Support output fields list customization
- New feature : Implement bridge support with new vmnet functionality
- New feature : Allow to claim/release USB devices by Serial Number
- New feature : Allow to select the bridge interface automatically
- New feature : MAC address sync
- New feature : Static IP addresses support (requires anka start -u for the VMs)
- New feature : Performance optimization
- Other : Remove vlaunch
- Other : Move ankanetd service to unix domain
- Other : Allow to create more than 4 anka vmnet interfaces
- Other : Integrate new VTUFS 3.10.4 (requires anka start -u for the VMs)
- Other : Changes to licenses

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.5.3 - Jan 10, 2020#
- Other : License changes

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.5.3 - Jan 10, 2020#
- Other : License changes

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.5.2 - Dec 08, 2019#
- Bug Fix : When selecting multiple nodes to change capacity from the UI, it doesn’t work

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.5.2 - Dec 08, 2019#
- Bug Fix : When selecting multiple nodes to change capacity from the UI, it doesn’t work

### Change log Jenkins Plugin version 1.22.2 - Dec 08, 2019#
- Bug Fix : Jenkins gets an exception while trying to start a new slave with OR operator

### Change log Packer Plugin version 1.1.0 - Dec 08, 2019#
- New feature : Add hyper-threading support for Packer plugin
- New feature : Add verbose output while creating image
- New feature : Integrate packer plugin with ansible

### Change log GitLabCI Plugin version 0.6b - Dec 08, 2019#
- New feature : update gitlab runner to new gitlab codebase

### Change log Jenkins Plugin version 1.22.1 - Nov 15, 2019
- Bug Fix : ankaGetSaveImageResult does not work for jobs in folders
- Bug Fix : Saving new cloud configuration would crash jenkins if controller does not support save image

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.5.0 - Nov 05, 2019
- Bug Fix : ankacluster status thinks node is joined when it is not.

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.5.0 - Nov 05, 2019
- Bug Fix : ankacluster status thinks node is joined when it is not.

### Change log Jenkins Plugin version 1.22.0 - Nov 05, 2019
- New feature : Adjust Jenkins Anka build plugin to "Configuration as code" plugin
- New feature : Implement dynamic slaves in jenkins anka build plugin
- Bug Fix : Cant access jenkins configuration page if controller is offline

### Change log TeamCity Plugin version 1.7.0 - Nov 05, 2019
- Bug Fix : Remove extra log messages in TC plugin

### Change log GitLab Intg - Nov 05, 2019
- Bug Fix : Gitlab runner requests vm with "empty" tag

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.4.0 - Oct 18, 2019
- Bug Fix : Reverted "Fixed controller handling of instances failing after start. User reported issue."
- New feature : Client-side load balancing for controller/registry

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.4.0 - Oct 18, 2019
- Bug Fix : Reverted "Fixed controller handling of instances failing after start. User reported issue."

### Change log Jenkins Plugin version 1.21.0 - Oct 18, 2019
- New feature : Add a "post build step" to check "save image request" status
- Bug Fix : cache builder - JNLP, Suspend setting doesn't work

### Change Log Anka 2.1.2 change - Oct 10, 2019
- Bug Fix : SSH is not turned ON by default on Catalina guests

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.3.1 - Sept 22, 2019
- Bug Fix : Fixed controller handling of instances failing after start. User reported issue.

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.3.1 - Sept 22, 2019
- Bug Fix : Fixed controller handling of instances failing after start. User reported issue.

### Anka 2.1.1 change - Sep 25, 2019
- Bug Fix : networking in Anka VMs doesn't work in Catalina beta 8. User reported issue.
- Bug Fix : License auto renewal doesn't work.
- Bug Fix : anka run -N sometimes fail to wait for network properly.

### Anka 2.1.0 change - Aug 21, 2019
- Bug Fix : Starting VM with USB device could interfere with previously assigned device
- Bug Fix : Anka View doesn't allow to drag with mouse
- Bug Fix : Guest hangs after host sleep
- Bug Fix : Shared folders doesn't work on Catalina
- Bug Fix : Pasteboard policy isn't indicated in GUI
- Bug Fix : Anka Secure license is being processes with core rules
- Bug Fix : Kernel panic in VeertuNet
- Bug Fix : anka stop doesn't trim encrypted drives
- Bug Fix : Display doesn't return to full screen after restart
- Bug Fix : Pasteboard policy isn't indicated in GUI
- New feature : Support hot plug of USB devices
- New feature : Allow to specify relative paths in anka run -w option
- New feature : Allow to control pb paste and copy operations independently
- New feature : Updated EULA and acknowledgements

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.3.0 - Sept 16, 2019
- Bug Fix : Refreshing any portal page defaults to dashboard page
- New feature : When an instance is in error show the node where it failed in the in portal dahsboard
- New feature : changes required to combine slave template builder and anka Jenkins plugin in one plugin
- New feature : Implement pushing anka agent logs to the controller as part of log reporting
- New feature : Agent shuts down unmanaged running vms when joined to a cluster

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.3.0 - Sept 16, 2019
- Bug Fix : Refreshing any portal page defaults to dashboard page
- New feature : When an instance is in error show the node where it failed in the in portal dahsboard
- New feature : changes required to combine slave template builder and anka Jenkins plugin in one plugin
- New feature : Implement pushing anka agent logs to the controller as part of log reporting
- New feature : Agent shuts down unmanaged running vms when joined to a cluster

### Change log Jenkins Plugin version 1.20.0 - Sept 16, 2019
- New feature : Combine slave template builder and anka Jenkins plugin in one plugin
- Bug Fix : Cancelling Jenkins job doesn't terminate the controller instance request
- Bug Fix : Keep Alive on Error" for a label for Jenkins Pipeline is not working

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.2.1
- Bug Fix : Registry has orphan .ank files after executing multiple "reverts"

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.2.1
- Bug Fix : Registry has orphan .ank files after executing multiple "reverts"

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.2.0
- Bug Fix : etcd high CPU usage reported by user
- New feature : Automatic upgrade anka agent included in Anka Binary, if there's a newer controller version
- New feature : Controller while spinning VMs on nodes dynamically changes node capacity based on concurrent vCPU/physical cores formula
- New feature : remove -g from ankacluster and add it as default
- New feature : Updated EULA and acknowledgements

### Change Log Anka Controller & Registry combined package (Mac) - Version 1.2.0
- Bug Fix : etcd high CPU usage reported by user
- New feature : Automatic upgrade anka agent included in Anka Binary, if there's a newer controller version
- New feature : Controller while spinning VMs on nodes dynamically changes node capacity based on concurrent vCPU/physical cores formula
- New feature : remove -g from ankacluster and add it as default
- New feature : Sign and notarize controller mac package
- New feature : Updated EULA and acknowledgements

### Change log Jenkins Plugin version 1.19.1
- New feature : handle reference to non-existent tag in registry from Jenkins plugin gracefully
- Bug Fix : Support ssh slaves 1.30.0 in jenkins Plugin
- Bug Fix : Jenkins pipeline job doesn't terminate slaves after finishing
- Bug Fix : Plugin not taking slave name field for JNLP

### Change log Jenkins Slave template Builder plug-in version 1.6.0
- Bug Fix : Handle the scenario of special char/whitespace in tag name in slave template prepare plugin
- Bug Fix : Support ssh slaves 1.30.0 in jenkins Plugin
- Bug Fix : subfolder structure in jenkins job causes salve template builder plugin to fail due to special char

### Change log TeamCity Plugin version 1.6.0
- Bug Fix : handle reference to non-existent tag in registry from TC plugin gracefully

### Change log GitLab CI Plugin
- Note : No Changes

See upgrade notes at [here](https://ankadoc.bitbucket.io/#Upgrade Instructions and Notes)

## Old Change Logs
### Version 2.0.1
- Anka Version 2.0.1 - Supports multiple license types for different products. Anka Build Basic, Anka Build Enterprise, Anka Build Enterprise Plus, Anka Flow, Anka Secure
- Anka Controller & Registry combined Linux package - Version 1.1.0
- AnkaControllerRegistry Mac package - Version 1.1.1
- Anka Jenkins plug-in - version 1.19
- Anka Teamcity plug-in - version 1.5

### Anka 2.0.1 change
- Bug Fix : Catalina beta 3 reboot/start error on anka create
- Bug Fix : Anka Flow license uses core based fulfillments
- Bug Fix : anka license show crashes
- Bug Fix : Same IP address is being assigned to bridged VMs
- Bug Fix : mmap files are not being deleted
### Anka 2.0 change
- Bug Fix : Sometimes VM fails to resume with error accessing state file
- Bug Fix : Bad serialization to YAML
- Bug Fix : Suspend failed
- Bug Fix : Anka view crashes on copy/paste operation
- Bug Fix : Hang on resume
- Bug Fix : macOS boot stuck sometimes
- Bug Fix : Problem in image/state deletion
- Bug Fix : Unaligned disk access
- Bug Fix : Guest kernel panic
- Bug Fix : Anka View Crash
- Bug Fix : libosxfuse has loading problems due to Library Validation
- Bug Fix : RealVNC client crashes ankahv
- Bug Fix : Berry doesn't load on 10.12
- Bug Fix : Guest kernel panics on resume
- Bug Fix : Can't attach more than one USB device
- Bug Fix : Uhost craches sometimes leaving the device busy
- Bug Fix : Ankahv crashes on sharedfs operation
- Bug Fix : subsequent clone/delete commands leave unreferenced state files
- Bug Fix : User reported permission problem in anka create -a
- Bug Fix : Tar fails to update modification date of symlinks over sharedfs
- Bug Fix : Guest reboots during the installation of Homebrew
- Bug Fix : During CI it's observed an increased number of guest panics
- Bug Fix : Anka registry add doesn't work with self-signed certificates
- Bug Fix : Ankahv crashes in xhci module
- Bug Fix : Deadlock on boot
- Bug Fix : Anka exception on registry operation
- Bug Fix : Anka trust insecure registry
- Bug Fix : Inefficient suspend
- Bug Fix : 1.4 suspended Vm fails to start in 1.5(internal release)
- Bug Fix : Anka view on 1.4 VM in 1.5(internal release) shows a black border at the bottom
- Bug Fix : VM became stopped on suspend
- Bug Fix : Anka usb list fails on new MBP2018
- Bug Fix : Installation of macOS guest hangs in the middle
- Bug Fix : Ios emulator fails to run
- Bug Fix : Ios emulator causes a triple fault with nAnkaVM driver loaded and nanka engine
- Bug Fix : Double cursor is displayed in anka view
- Bug Fix : Anka vm doesn't have networking after first boot (only after a reboot)
- Bug Fix : Long running VMs crashing (probably due to resources/memory overallocation)
- Bug Fix : Policy functionality is broken in very latest versions of Anka (Anka Secure Product related)
- Bug Fix : Anka does not start the graphics driver with correct arguments
- Bug Fix : Anka clone operation doesn't check for policy rules producing unstartable VMs in negative scenario (Anka Secure Product related)
- Bug Fix : Guest macOS v10.12 hangs on boot on Berry load
- Bug Fix : License activation detecting 8core imac as 4 core
- Bug Fix : After creating sierra VM network driver not loaded
- Bug Fix : Enable policy without forcing user to stop VM (Anka Secure Product related)
- Bug Fix : Vcpu_lock() deadlock
- Bug Fix : Anka registry pull fails due to missing of policy file (Anka Secure Product related)
- Bug Fix : Anka registry add doesn't work with certificates
- Bug Fix : Latest controller fails to start VM
- Bug Fix : Issue with the registry for mac summary window during install
- Bug Fix : Anka registry does not return right error for 403
- Bug Fix : Swift build fails over a shared folder
- Bug Fix : Switching to Keylocked Anka View (with e.g. Cmd+Tab) leaves the keys pressed on host side time to time
- Bug Fix : osxfuse 3.3 (used currently) is not fully compatible with 10.14
- Bug Fix : Uhost fails to pass-through some USB devices
- Bug Fix : Anka View (2.0) crashed during live resize
- Bug Fix : Host not reporting CPu core count during renewal/license exp call. previous core count is being overwritten.
- Bug Fix : Installer hangs on 10.15 beta
- Bug Fix : Ankactl crashes on start, stop, show operations on 10.15 beta
- Bug Fix : AnkaView shows white box in 10.15 beta
- Bug Fix : Anka create fails to create/configure the default user
- Bug Fix : Image that was suspended for longer than 1 hour does not work in 10.15 beta VMs.
- Bug Fix : "Anka View quit unexpectedly" when I turned off the VM through the Apple menu
- Bug Fix : Failed to assign 256 bytes of key data, error 0
- Bug Fix : Pull operation doesn't update policy file reference in VM object (Anka Secure Product related)
- Bug Fix : Fix typos
- Bug Fix : Long boot of 10.15 beta
- Bug Fix : Anka --debug registry pull --shrink issue
- Bug Fix : Bad messaging for registry operations if no registry configured
- Bug Fix : Better UID/permissions support in shared folders
- New feature : Massive performance optimizations
- New feature : Catalina VM support
- New feature : Secure controllable VM environments (Anka Secure Product related)
- New feature : New AnkaView application with multi-display support
- New feature : Unified Anka package
- New feature : Allow extending existing ANKA images
- New feature : New anka view screenshot command form
- New feature : Disable network discovery to prevent duplicate name dialogs
- New feature : Detect ssh session in anka view and inform the user about vnc
- New feature : Limit Anka View window size by desktop visible area
- New feature : Allow connecting multiple USB devices to VM
- New feature : Better TLS support in anka registry 
- New feature : Ability to encrypt VNC password
- New feature : OpenID connect login support for anka registry
- New feature : Show creation/start/stop dates of VM
- New feature : Added ability to passthru/configure PlatformID and Serial Number to the VM 
- New feature : Show  license fulfillment information associated with a key with anka license command

### Change Log Anka Controller & Registry combined package (Linux) - Version 1.1.0
- Bug Fix : 1.18 Jenkins plugin doesn't respect "keep alive on failure" flag
- Bug Fix : Teamcity plugin incompatible with version 8.1.1
- Bug Fix : Distribute template sometimes doesn't receive process report from agent although process succeeded
- Bug Fix : Delete permission group doesn't delete the group on registry
- Bug Fix : Controller showing incorrect license type on dahsboard
- Bug Fix : Anka registry leaves unreferenced image files and deleted vm folders on the disk
- Bug Fix : Registry/revert api deletes the entire Vm when version specifies doesn't exist
- New feature : Removed beanstalk and implemented custom queue
- New feature : Move mgmt portal default to port 80
- New feature : Move registry defaukt port to 8089
- New feature : Add super user authentication support for controller
- New feature : Provide a way to take Node offline through controller UI and REST API (without disjoining)
- New feature : Move controller docker base images to CentOS7
- New feature : Prevent excessive logging in anka agent
- New feature : EXpose tag deletion capability through Controller REST API
- New feature : Add interactive/non-interactive (openid) authentication support to anka client pkg
- New feature : Expose Portal ui functionality according to user permissions
- New feature : Add configuration options for event logging
- New feature : Add automatic tls behaviour to ankacluster connection test
- New feature : Admin Ui for controller
- New feature : SSO Support with openid
- New feature : Certificate-based authentication to controller and Registry
- New feature : Add Node IP aliasing
- New feature : Add USB REST APIs
- New feature : New configuration and advanced configuration files
- New feature : Configuration parameter to store groups data(etcd) location

### Change log Jenkins Plugin version 1.19
- New feature : Add Eterprise tier authentication/authorization support in Jenkins Plugin to access Controller
- New feature : Changes to plugin connectivity to controller new configuration parameters

### Change log TeamCity Plugin version 1.5
- New feature : Add Eterprise tier authentication/authorization support in TeamCity Plugin to access Controller

### Upgrade Notes
- Note     : There is no need to upgrade Anka VM created with 2.0 version to version 2.1.
- Required : Shared folder is not supported for Catalina Anka VMs. We are waiting for Fuse libraries to be updated for Catalina.
- Required : This release requires upgrade of Anka package on all of your nodes, controller, registery and all plugins.
- Required : This release requires upgrade of all existing 1.4.X VM templates. Use anka start -u VMName/VMUUID command.
- Required : Version 1.19 of Jenkins Plugin is not fully compatiable with older version sof controller 1.0.14 or 1.0.14.1. If you upgrade your plugin to version 1.19, you will need to recreate your Anka Cloud configuration in Jenkins.
- Required : Https registries have to be re-added in 2.0.



## Change Log version 1.4.4
### Anka 1.4.4 change
- Bug Fix : fixed the occassional deadlock on boot issue
- Bug Fix : fixed shared fs system issue

## Anka Build (supported on all mac hardware including 2018 Mac Mini)
### Version 1.4.3
- Anka Build - Version 1.4.3
- Anka Controller & Registry combined package - Version 1.0.14
- Anka Controller & Registry combined Mac package (This pkg is meant for users to quickly deploy Controller and Registry on Mac.) - Version 1.0.14	
- Anka Jenkins plug-in - version 1.18
- Anka Teamcity plug-in - version 1.4

## Anka Flow (supported only on macbook Air, macbook Pro, iMac)
### Version 1.4.3
- Anka Flow - Version 1.4.3

## Change Log version 1.4.3
### Anka 1.4.3 change
- Bug Fix : anka modify vmname set cpu issue
- Bug Fix : anka create with custom user/password not working with mojave VMs
- Bug Fix : anka start -u changes the currently logged in user to anka user
- Bug Fix : Problems passing through iOS 12 devices
- Bug Fix : macOS timing is incorrect on VMs that are resumed on different type of machine than the machine that used to suspend
- Bug Fix : VeertuBus doesn't boot in macOS 10.12
- Bug Fix : Update copyright string to 2019
- Bug Fix : anka license activate consumes fulfillment even on non-eligible machines
- Bug Fix : workaround for excessive logging when nodes can't connect with controller service
- New feature : Core based licensing
- New feature : Support for nested VM to run Docker inside Anka VM (Vm with nested flag enabled should be created with a minimum of 4vCPU, 4GB RAM)
- New feature : Support to activate Anka Basic, Enterprise License Tiers
- New feature : Improve anka run profile sourcing options. Added support for the ability to source a profile file from anka run - anka run vmname source /source_file_path



## Change Log Anka Controller & Registry combined package - Version 1.0.14
- Bug Fix : Template in registry contained an empty anka block file
- Bug Fix : Instance pulling state should show node details
- Bug Fix : Registry on mac not working correctly because Finder creates temp files in the storage folder
- New feature : Implemented support for Basic and Enterprise Tier
- New feature : Add version of the controller and registry on the portal page
- New feature : Expose Controller logs through the portal

## Change Log Jenkins Plugin - Version 1.18
- New feature : Improve Jenkins/controller logs
- New feature : Expose priority through Jenkins Plugin
- New feature : Add field in Jenkins to specify optional java params for ssh launcher

## Change Log TeamCity Plugin - Version 1.4
- New feature : Expose groups and priority through Plugin


### Version 1.4.2
- Anka Build - Version 1.4.2
- Anka Controller & Registry combined package - Version 1.0.13
- Anka Registry Mac package (This pkg is meant for users to quickly deploy Registry on Mac for testing purposes.) - Version 1.0.11	
- Anka Jenkins plug-in - version 1.17
- Anka Teamcity plug-in - version 1.3


## Anka Flow (supported only on macbook Air, macbook Pro, iMac)
### Version 1.4.2
- Anka Flow - Version 1.4.2

## Change Log version 1.4.2
### Anka 1.4.2 change
- Bug Fix : Xcode 10 random crashes
- Bug Fix : anka create - error on reboot
- Bug Fix : User reported issue with anka create -c
- New feature : Additional flag in anka license activate command


### Anka 1.4.1 change
- Bug Fix : Fixed networking issue during anka create for Mojave VM
- Bug Fix : Fixed anka create -s flag not working
- Bug Fix : Fixed incorrect pull progress
- Bug Fix: Fixed possible data corruptions during TRIM
- Bug Fix: Fixed issue of upgrade from 10.14 to 10.14.1

## Change Log Anka Controller & Registry combined package - Version 1.0.13
- Bug Fix: Fixed VM template not showing actual tag for when latest is selected

## Anka Build (supported on all mac hardware)
### Version 1.4
- Anka Build - Version 1.4
- Anka Controller & Registry combined package - Version 1.0.12
- Anka Registry Mac package (This pkg is meant for users to quickly deploy Registry on Mac for testing purposes.) - Version 1.0.11	
- Anka Jenkins plug-in - version 1.17
- Anka Teamcity plug-in - version 1.3


## Anka Flow (supported only on macbook Air, macbook Pro, iMac)
### Version 1.4
- Anka Flow - Version 1.4

## Change Log version 1.4
### Anka 1.4 change
- New feature : Added support of APFS guests (macOS 10.13+)
- New feature : Optimized CPU consumption in Anka View (up to 50%)
- New feature : Configurable port forwarding auto-assignment range
- New feature : Added local mode of push/pull operations for local version management
- New feature : Added guest public mechanism (BSD Notification) to run actions on resume
- New feature : Added protection from “early suspends” that could be a reason of hanged session
- New feature : Added VM name to corresponding logs to simplify log analysis
- New feature : Added ability to push VM as new tag of another VM in registry
- New feature : Better compatibility with JEDEC and IEK in size parameters of anka CLI
- New feature : Better EFI error handling
- New feature : Added ability to reference USB devices by user assignable claim name
- New feature : Added support of disk resizing
- Bug Fix : Fixed anka list inoperable if one of VM in the list corrupted
- Bug Fix : Fixed push/pull of VM with several drives could lead to corrupted VM
- Bug Fix : Fixed anka clone -c doesn’t work
- Bug Fix : Fixed VM support with more than 34GB
- Bug Fix : Fixed VM resume with USB device could fail
- Bug Fix : Fixed anka config doesn’t apply non-string parameters
- Bug Fix : Fixed anka create could fail due to problem in emulation logic
- Bug Fix : Fixed anka show could improperly determine IP address of the hos
- Bug Fix : Fixed clone operations of VMs with nested virtualization enabled
- Bug Fix : Fixed elevated CPU consumption due to Docker nested virtualization in Anka VM
- Bug Fix : Improvements in anka run stability
- Bug Fix : Fixed simultaneous anka start -u operations could fail
- Bug Fix : Fixes in suspend procedure that could lead to frozen guest after resume
- Bug Fix : Fixed anka license activate problem in some proxied networks
- Bug Fix : Increased security of privileged agent


## Change Log Anka Controller & Registry combined package - Version 1.0.12
- New feature : Display error logs for VM provisioning in the controller portal
- New feature : Distribute to a specific Node from the portal
- New feature : Add a column in controller instance page to show the host and also tag for the template being used for provisioned VM
- New feature : Updated Controller acknowledgement document
- Bug Fix : Fixed ankacluster join -m flag
- Bug Fix : Fixed registry possible corruption issue when push operation is interrupted
- Bug Fix : Fixed ankaregctl error in output
- Bug Fix : Fixed push operation registry doesn't show template issue
- Bug Fix : Fixed controller agent issue when using tls for queue connection
- Bug Fix : Fixed ankacluster disjoin sometimes leaving nodes in the controller view

## Change Log Jenkins Plugin version 1.16
- New feature : Support JNLP based connection
- Bug Fix : Fix for ClassCastException
- Bug Fix : Terminate VM provision request stuck in schd state for more than an hour

## Change Log Jenkins Plugin version 1.16
- New feature : Jenkins plugin timeout should be increased from 60 seconds to 120 seconds
- New feature : Implement support for node groups in Jenkins Plugin. Node Groups will be available in future release of Controller
- New feature : Change the label "Build Controller URL" in Jenkins plugin Ui to "Build Controller URL with port"
- New feature : Jenkins plugin timeout increased from 120 seconds to 240 seconds

## Change Log TeamCity Plugin version 1.3
- New feature : Implement support for node groups in the Plugin. Node Groups will be available in future release of Controller

> Note - If you are moving to version 1.4 from previous versions, upgrade your controller, registry, Jenkins Plugin, Anka Build.pkg and Anka Flow.pkg. Also, you will need to upgrade guest add-ons on your existing VM templates.

## Change Log version 1.3.3
### Anka 1.3.3 change
- Bug : AnkaBuild.pkg & AnkaFlow.pkg - The network link is shown as inactive although there's an active IP address and connectivity. This causes some network settings like system proxy or custom dns not to work properly - Resolved
- Bug : Anka Build Controller - Minor fixes to pull state logic - Resolved
- Bug : Jenkins Plugin - Fixed issue with resolving pulling and schedulding states in jenkins Plugin - Resolved
- New feature : AnkaBuild.pkg & AnkaFlow.pkg - Users need to configure guest environment of anka run execution. Support added for .profile files.
- New : AnkaBuild.pkg & AnkaFlow.pkg - Purge history for the VM on the node, except the current version which was pulled.
- New : Jenkins Plugin - Support for JNLP
- New : Teamcity Plugin - Support for https setting of controller

## Change Log Jenkins Plugin version 1.15
- Bug :Jenkins Plugin - Not able to start Vm of a particular template/tag when ssh is selected - Resolved
- New : Jenkins Plugin - Introduced node groups field. Backend support wil be available in upcoming next release

## Change Log Jenkins Plugin version 1.14
- New : Jenkins Plugin - Changed timeout from 60s to 120s

## Change Log Jenkins Plugin version 1.13
- New : Jenkins Plugin - Support for JNLP

## Change Log Jenkins Plugin version 1.12
- Bug :Jenkins Plugin - Pipeline Jenkins jobs continue to use the same instance without deleting the instance - Resolved
- New : Jenkins Plugin - Support for https setting of controller

## Change Log version 1.3.1
> Note - Version 1.3.1 is a minor release over version 1.3. If you are moving to version 1.3.1 from 1.3, you only need to upgrade the Anka Build.pkg and Anka Flow.pkg. There is no need to upgrade controller and registry. If you are moving to version 1.3.1 from a version older than 1.3, then upgrade all components including controller, registry, plugins and your existing VM templates.
### Anka package 1.3.1 change
- Bug : anka vm can't use more than 2GB ram due to hfs changes introduced in version 1.3 - Resolved
- Bug : Custom user created during anka create display anka as the real name instead of user name - Resolved
- Bug : anka create fails with Unrecognized Date Format on some machines - Resolved
- Bug : anka start command never finishes in some scenarios - Resolved
- Bug : anka create hangs, anka view shows hang at macOS install screen - Resolved
- New feature : Added message in anka license remove to show backend license fulfillment id.
- New feature : anka license activate command stores the CPU core count in licensing backend

## Change Log version 1.3
### Anka package 1.3, Anka Controller & Registry 1.0.8 change
- Bug : Upgrade of macOS is broken in 1.3 - Resolved
- Bug : VM often exits on reboot with "invalid vmcs" status - Resolved
- Bug : Starting anka with -d changes static configuration - Resolved
- Bug : anka registry push command enables push of Vms with same names - Resolved
- Bug : start-stop sequence works bad - Resolved
- Bug : anka mount operation hangs on permission denied error - Resolved
- Bug : ankactl crash - Resolved
- Bug : anka create runs successfully but returns action failed. The Vm is created and is in stopped state. - Resolved
- Bug : running anka run vmname ping 8.8.8.8 and doing suspend in different terminal to the same vm result in -anka:mount error 17 next resume - Resolved
- Bug : ankaupd respawns many times due to too soon exit if no updates were found - Resolved
- Bug : anka registry pull/push commands are not synchronized - Resolved
- Bug : Debug output of uhost - Resolved
- Bug : Invalid JSON output of anka registry list-repos - Resolved
- Bug : ankactl error messages shouldn't contain new line characters - Resolved
- Bug : typos in anka help - Resolved
- Bug : anka run -f doesn't override guest environment, but should - Resolved
- Bug : unfriendly exception on pushing running VM - Resolved
- Bug : 'ls' operation over shared system leads to hangs sometime (3-4% of cases) - Resolved
- Bug : anka mount hangs on "permission denied" case, but should return error - Resolved
- Bug : sudo with anka registry - sudo anka registry push 133b387 uitest Password: -anka: 'NoneType' object has no attribute 'prepare_multipart_head_request' - Resolved
- Bug : Crash in anka_run - Resolved
- Bug : anka --machine-readable registry list-repos fails when there are no repos configured - Resolved
- Bug : -anka: Conversion failed when disk was mounted - Resolved
- Bug : Controller portal table fields are too narrow - Resolved
- Bug : anka modify VM add optical-drive $PATH doesn't recognize relative paths - Resolved
- Bug : Error while deleting network-card - Resolved
- Bug : Physical removal of claimed USB device leaves zombies - Resolved
- Bug : pci_net (not VeertuNet) module hangs on guest reboot/shutdown - Resolved
- Bug : Disable allowing negative nos in number selector VM capacity field in portal - Resolved
- New feature : Extend StartVM controller API
- New feature : package nanka
- New feature : Return UUID in anka pull, like in anka create/clone
- New feature : Support Sierra guest VM with new 1.3 anka create a method
- New feature : Prevent "Same name already exists" network dialogs
- New feature : Enable changing max vm value for anka build nodes from the controller portal
- New feature : self-license reactivation
- New feature : Improve working with system domain in ankactl
- New feature : Replacing Anka create macOS installer with iso installer for automatic installation of guest
- New feature : Revert spotlight disabled by default. Spotlight is now enabled by default.
- New feature : Upgrade suspended guests with single start -f -u command
- New feature : Add (beta) word to description of set nested command
- New feature : Display error messages in controller when instance is in error state
- New feature : Replacing Anka create macOS installer with iso installer for automatic installation of guest
- New feature : Do not return error on suspending VM that is already suspended
- New feature : Allow to configure user name for anka create -a procedure
- New feature : Allow to select registry repo with --remote option
- New feature : Integrate network wait functionality into anka run
- New feature : add check in anka create to avoid user specifying too small values for RAM, disk size
- New feature : add a generic message about approx. time for anka create
- New feature : Terminate anka run on second SIGINT
- New feature : Display error in anka create when user gives too less disk space in the command
- New feature : Special handling of EBUSY error on unmount
- New feature : Add progress for image distribution/pull state
- New feature : Add sort option for controller portal tables
- New feature : Add pull state with progress as instance state in controller
- New feature : Introducing more state for dashboard reporting other than scheduling
- New feature : dashboard should show when hosts are not activated
- New feature : Allow running agent on imaged machine without need to login into UI
- New feature : check for active/inactive license in anka create in the beginning and display message
- New feature : Sort vm list by name, not uuid
- New feature : Unify registry push and pull syntax
- New feature : Verify registry connectivity in anka registry add command

#### DIFF in anka CLI version 1.3
- anka license set - Deleted
- anka modify set nested - New
- anka modify set memory-prefetch - New
- anka run -N, --wait-network - New
- anka registry -r - New

### Anka package 1.2.2 change
- Fixed bug with sudo anka --debug license command.
- Fixed bug with anka unmount hangs on parallel operations till all commands end.
- Fixed bug with dmg2anka.
- Increase the frequency of anka agent updates to controller.
- Allow configurable timeout for anka commands.
### Registry 1.0.7 changes
- Fixed issue with anka registry version reporting.
- Fixed issues with pushing VM with same tag to the registry.
- Fixed issue with Mac registry package doesn't create the registry folder by itself.
- Fixed issue with Incorrect error message "could not connect to server" when registry is short on space.
- Introduced change to allow user to pull VM with same name as what�s existing on their machine.
- Added ability to add tag description during registry push (reuses VM description field).
### Controller 1.0.7 changes
- New management portal available in controller to view status of Anka Build cloud. See documentation for more detail.
- Fixed the bug with controller agent may return wrong anka build node ip address.
- New controller REST API function to force delete anka build node.
- Introduced field for slave name template in Jenkins plugin.
- Combined controller and registry Docker packages into single package for download.### Anka package 1.2.1 change
- Reverted Spotlight is disabled in Vms created with anka create -a ... command.
- Fixed issue with anka command line module impacting pulling different versions of VM from the registry.
### Anka package 1.2 changes
- Automatically validate consistency of Anka and VM addons version. Check upgrading anka documentation for mroe details 
- Added protection from unsolicited execution of anka commands with sudo.
- Improved VM creation with ISO procedure. See documentation on creating Vm from ISO for more detail.
- Optimize VM creation/upgrade procedures for instant start.
- Fix guest OS upgradability.
- changed the /tmp to /var/tmp for the shared memory video.
- Fixed the bug with anka clone creates default network-card always.
- Fixed the bug with user logout from inside the VM shutdowns the VM and in some cases restarts the VM.
- Implemented support for 18vcpu Anka VMs.
- Spectre and Meltdown related changes.
- Fixed bug with ankacluster command not reporting right version.
- Fixed issue with anka delete fails in certain scenarios.
- Fixed issue with the license is valid, it doesn't output as JSON.
- Fixed issue with anka usb list --machine-readable crashes.
- Fixed issue with ankahv process leave temporary files.
- Fixed issue with anka run fails to set workdir on already mounted folder.
- Fixed issue with xcodebuild with anka run fails after some time with Bad file descriptor error.
- Fixed issue with issue with Anka Build interactive priority.
- Fixed issue with old artefacts from modify network-card command.
- Spotlight is disabled in Vms created with anka create -a ... command. This is done to make the VMs execute faster for CI jobs. See documentation on creating VMs for more details.
- Implemented new block shrink logic to optimize VM footprint.
- Fixed issue with license activation changed permissions of the log file.
- Fixed issue with anka.log file size infinite growth.
- Fixed issue with Deadlock in PCI related modules on resume, reboot and shutdown.
- Fixed issues with anka delete sometimes failing.
- Extended syntax for stop, delete, suspend operations: accepts multiple VMNAME and --all parameters.
- Modified anka create to create macOS VMs with SIP/Kext Consent disabled by default. See documentation on creating VMs for more details.
- Fixed issues with anka run hanging on (abnormally) terminated VM.
- Removed anka modify headless flag.
- Prevent VM name duplicates with anka create and anka clone commands.
- Optimized anka run execution to allow to "reuse" existing mount points (nested mount).
- Introduced change to make Anka home as default folder for ankarun.
- Introduced mounting external drives and USB devices without writing them to the VM config.
- Fixed SMC hang in High Sierra shutdown.
- Optimize suspend/resume performance.
- Optimization in block IO leading to “some improvements” in compillation of “some” projects


