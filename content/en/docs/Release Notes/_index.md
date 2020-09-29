---
date: 2018-03-06
title: "Release Notes"
linkTitle: "Release Notes"
weight: 7
description: >
  Detailed Release Notes
---

> Not all plugins are maintained by Veertu Inc developers. You might not see them listed here.

## Current Versions

### Anka Build Cloud Controller & Registry 1.11.0 (1.11.0-59d63cca) - August 31, 2020
- New Feature : `anka-controller` executable from macOS native package now supports `--tail {lines}`
- Improvement : Removed all unnecessary files from docker .tar.gz archive
- Bug Fix : Pulling a template/tag now calculates the size of the tag properly (instead of only the latest tag for the template)
- Bug Fix : VNC url in Controller UI > Instances page is now a link again
- (Standalone Registry: 1.11.0-d43f91c)

### Anka Virtualization CLI 2.2.3 (2.2.3.118) - May 03, 2020
- Bug Fix : Starting multiple VMs from suspended state with bridge networking configuration
- Bug Fix : Unable to set up port forwarding to 127.0.0.1
- Bug Fix : VNC connection error
- Bug Fix : DHCP bug related to renew lease in some Anka environment
- Bug Fix : anka VMs can get corrupted when using nanka kext in cases of force shutdown
- Bug Fix : Trying to start a VM result in failed state - with logs saying bind: Address already in use
- New feature : When using Anka Viewer, after clicking the green full screen button on the window's top bar, it's unclear how to get out of full screen. Now, if you move your mouse to the top of the screen you will expose the bar and green button.
- New feature : Updated license terms

    > There is no requirement to upgrade the VM templates from previous anka version 2.2.2 to version 2.2.3.

### TeamCity Plugin version 1.7.1 - July 7, 2020
- Bug Fix : Long-running threads were being created
- Bug Fix : UI Slowness the more Instances/Agents you created
- Bug Fix : HTTPS without certificate authentication enabled doesn't work

### Jenkins Plugin 2.2.0 - August 31, 2020
- Bug Fix : vmPollTime definitions in Configuration as Code aren't working due to typo

    > **If updating from 1.X.X: This version (2.X) requires that you back up your Static Slave Templates (config.xml) and add them again.**

### Anka VM GitHub Action v1.2.0-beta - June 17, 2020
- Beta released to public

### Packer Plugin 1.5.0 - August 31, 2020
- New feature : Added ability to modify cpu core count, ram, and disk size when cloning from an existing VM Template

### Anka GitLab Runner 1.1.0 - Sep 28, 2020
- Feature Change : When limiting jobs to a specific node using the enterprise license feature "node groups", you no longer use the `GROUP_ID` ENV and instead set it with `--anka-node_group` on registration or the ENV `NODE_GROUP`.

---

## Previous Versions

### Anka GitLab Runner 0.6b - Dec 08, 2019
- New feature : update gitlab runner to new gitlab codebase

### Packer Plugin 1.3.0 - June 18, 2020
- New feature : Properly handle cleanup of VM when halted or cancelled

### Jenkins Plugin 2.1.2 - July 8, 2020

> **If updating from 1.X.X: This version (2.X) requires that you back up your Static Slave Templates (config.xml) and add them again.**

- Bug Fix : createDynamicAnkaNode remoteFS and launchMethod are being ignored
- Bug Fix : HTTPS without certificate authentication enabled doesn't work

### Jenkins Plugin 2.1.0 - June 22, 2020

> **If updating from 1.X.X: This version (2.X) requires that you back up your Static Slave Templates (config.xml) and add them again.**

- Various stability / performance improvements
- New feature : Node/slave name and jenkins url is passed to the controller to display within the instances page
- New feature : A warning will display when users upgrade within the plugin center
- New feature : Ability to set an instance cap per Static Slave Template or per Anka Cloud
- Bug Fix : VMs were being left in started stage after job completed/aborted in Jenkins
- Bug Fix : When cache building, "Checking save image status" would immediately return success and the Job would complete even though the cache tag was still being pushed.

### Anka Build Cloud Controller & Registry 1.10.1 - August 20, 2020
- New feature : [Unresponsive VM monitoring with `ankacluster join --enable-vm-monitor`]({{< relref "docs/Whats New/_index.md#unresponsive-vm-monitor" >}})

### Anka Build Cloud Controller & Registry 1.10.0 - August 5, 2020
- Bug Fix : Central Logging was preventing nodes from joining
- Bug Fix : Certificates with a space in the Organization (O=) were not supported when trying to set permission groups.
- Bug Fix : Controller crashes if client tries to show permission group after deletion

### Anka Build Cloud Controller & Registry 1.9.1 - June 23, 2020
- Bug Fix : When a user sets a custom img_lib_dir location, the anka agent doesn't calculate disk usage for the new location

### Anka Build Cloud Controller & Registry 1.9.0 - June 10, 2020
- New Feature : Management of MAC Addresses to prevent rare collision
- New Feature : MAC Address can be specified when creating a VM using the API

### Anka Build Cloud Controller & Registry 1.8.0 - June 16, 2020
- New Feature : Controller REST API now supports modifying the vCPU and RAM for VMs in a stopped state
- New Feature : Controller REST API Start VM Instance object allows for custom key/value metadata
- New Feature : [Controller Instances page now allows users to add custom columns (from metadata key/value)]({{< relref "docs/Whats New/_index.md" >}})
- New feature : Updated license terms
- Bug Fix : Agent process stays alive despite disjoin
- Bug Fix : Instance sorting in controller UI was not kept on update
- Bug Fix : Sorting in instances page in dashboard is reversed
- Bug Fix : Agent's HTTP client would sometime fail to parse controller's responses

### Jenkins Plugin 1.23.0 - Mar 23, 2020
- Bug Fix : Controller IP is inaccessible from the Jenkins and Jenkins page is stuck on “loading” forever
- Issues in amazon-ecs-plugin version 1.26 cause problems with Jenkins-Anka Plugin. In order to fix this, downgrade to amazon-ecs-plugin version 1.22 or disable amazon-ecs-plugin. Check https://github.com/jenkinsci/amazon-ecs-plugin/issues/158 for more details.

### Packer Plugin 1.1.0 - Dec 08, 2019
- New feature : Add hyper-threading support for Packer plugin
- New feature : Add verbose output while creating image
- New feature : Integrate packer plugin with ansible

### Change Log Anka Build Cloud Controller & Registry combined package (Mac) version 1.7.1 - Apr 14, 2020
- Bug Fix : Anka agent miscalculates free space to keep when pulling images on the node

### Change Log Anka Build Cloud Controller & Registry combined package (linux) version 1.7.1 - Apr 14, 2020
- Bug Fix : Anka agent miscalculates free space to keep when pulling images on the node

### Anka 2.2.2 - Mar 03, 2020
- Bug Fix : Nested virtualization not working since rel 2.2.0
- Bug Fix : Anka command failed: time data ‘2020-02-27T03:52:23Z’ does not match format ‘%d %b %Y %H:%M:%S’
- Bug Fix : license pass-through from root VM to nested VM doesn't work
- Bug Fix : anka run sources both .bash_profile and .profile. Requires`anka start -u` for existing VM templates.
- Bug Fix : after rebooting a running vm with anka run VM sudo reboot`,  network doesn't work.
- New feature : Allow to specify display physical(DPI) parameters. Requires `anka start -u` for existing VM templates.

> There is no need to upgrade the VM templates from previous anka version 2.2.1 to version 2.2.2, unless you need to use the items from the above list which explicitly state the requirement of `anka start -u`.

### Anka Build Cloud Controller & Registry 1.7.0 - Mar 23, 2020
- Bug Fix : ankacluster doesn't pass max vm count to agent
- Bug Fix : Node upgrade request is sometimes sent more than once
- Bug Fix : Distribute not showing progress
- Bug Fix : Controller moves from ‘enterprise’ to ‘basic’ after uninstall and upgrade anka from 2.2.1 to 2.2.2
- New feature : Display node storage usage information in the controller dashboard
- New feature : Show size of templates in the controller dashboard
- New feature : Show a link to the jenkins job that the vm is running in the controller dashboard
- New feature : Put prompt while deleting template from the controller dashboard
- New feature : add reserve disk flag in ankacluster command
- New feature : Add the option to recover from a crash while uploading files to registry

### Anka Build Cloud Controller & Registry 1.6.0 - Mar 08, 2020
- Bug Fix : Jenkins plugin leaves zombie VMs after upgrade to version 2.204
- Bug Fix : Anka registry pull hangs forever when run from agent if VM has a lot of image files
- New feature : Handle vm start failures more robustly
- New feature : Added a fail timeout for template distribution

### Jenkins Plugin version 1.22.4 - Mar 08, 2020
- Bug Fix : Jenkins plugin leaves zombie VMs after Jenkins upgrade to 2.204
- Bug Fix : Jenkins leaves zombie VMs when restarting
- Bug Fix : Jenkins leaves zombie VMs when job has error on JNLP node
- Bug Fix : Jenkins Job changing Jenkins Master node labels
- Bug Fix : Jenkins JNLP Job starts more instances than it should
- Bug Fix : Jenkins config hangs of controller IP is inaccessible

***Note*** Currently issues in amazon-ecs-plugin version 1.26 cause problems with Jenkins-Anka Plugin. In order to fix this, downgrade to amazon-ecs-plugin version 1.22 or disable amazon-ecs-plugin. Check https://github.com/jenkinsci/amazon-ecs-plugin/issues/158 for more details.

### Anka 2.2.2 change - Mar 03, 2020
- Bug Fix : Nested virtualization not working since rel 2.2.0
- Bug Fix : Anka command failed: time data ‘2020-02-27T03:52:23Z’ does not match format ‘%d %b %Y %H:%M:%S’
- Bug Fix : license pass-through from root VM to nested VM doesn't work
- Bug Fix : anka run sources both .bash_profile and .profile. Requires`anka start -u` for existing VM templates.
- Bug Fix : after rebooting a running vm with anka run VM sudo reboot`,  network doesn't work.
- New feature : Allow to specify display physical(DPI) parameters. Requires`anka start -u` for existing VM templates.

### Anka 2.2.1 change - Feb 24, 2020
- Bug Fix : RFB server crash 0x0000000104fa0b8c rfb_thr + 1364
- Bug Fix : Crash on suspend
- Bug Fix : ankactl doesn't report vnc_password string
- Bug Fix : Anka VM fails to start under jenkins master account
- Bug Fix : Anka run --wait-network doesn't seem to work
- Bug Fix : set resolution from anka view doesn't work.
- Bug Fix : Anka version takes a few minutes to respond
- Bug Fix : ankactl experiences EMFILE if there are many snapshots/vms in the library
- Bug Fix : Adding MAC address using anka modify for bridge cards result in corrupted config file for VM
- Bug Fix : Collision of anka and guest addons files
- Bug Fix : VNC not working for when resolution of VM is changed to 1000*600
- Bug Fix : Problems to detect IP address
- Bug Fix : VM doesn't get unique IP address if static MAC address specified
- New feature : Support IPv6 in anka rfb server
- New feature : Allow to assign MAC address to VM
- New feature : Create directories if missing with anka run --workdir (requires anka start -u)
- New feature : Send HID events programmatically (requires anka start -u)
- New feature : Create RFB threads on demand only
- New feature : Give proper message in case of a VM trying to access out of range memory

### Jenkins Plugin version 1.22.3 - Jan 27, 2020
- Bug Fix : Jenkins takes nodes offline before restart
- Bug Fix : Jenkins logs Anka related messages when other agents (non anka) disconnects
- Bug Fix : Jenkins leaves zombie VMs when restarting

### Anka Build Cloud Controller & Registry (Mac) - Version 1.5.4 - Jan 15, 2020#
- Bug Fix : Version 1.5.3 was overwriting /usr/local/bin/anka-controllerd file

### Anka Build Cloud Controller & Registry (Linux) - Version 1.5.4 - Jan 15, 2020#
Other : Version number change to 1.5.4 to make it match with the mac package

### Anka 2.2.0 change - Jan 10, 2020
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

### Anka Build Cloud Controller & Registry 1.5.2 - Dec 08, 2019#
- Bug Fix : When selecting multiple nodes to change capacity from the UI, it doesn’t work

### Jenkins Plugin version 1.22.2 - Dec 08, 2019#
- Bug Fix : Jenkins gets an exception while trying to start a new slave with OR operator

### Jenkins Plugin version 1.22.1 - Nov 15, 2019
- Bug Fix : ankaGetSaveImageResult does not work for jobs in folders
- Bug Fix : Saving new cloud configuration would crash jenkins if controller does not support save image

### Anka Build Cloud Controller & Registry 1.5.0 - Nov 05, 2019
- Bug Fix : ankacluster status thinks node is joined when it is not.

### Jenkins Plugin version 1.22.0 - Nov 05, 2019
- New feature : Adjust Jenkins Anka build plugin to "Configuration as code" plugin
- New feature : Implement dynamic slaves in jenkins anka build plugin
- Bug Fix : Cant access jenkins configuration page if controller is offline

### GitLab Intg - Nov 05, 2019
- Bug Fix : Gitlab runner requests vm with "empty" tag

### Anka Build Cloud Controller & Registry (Mac) - Version 1.4.0 - Oct 18, 2019
- Bug Fix : Reverted "Fixed controller handling of instances failing after start. User reported issue."
- New feature : Client-side load balancing for controller/registry

### Anka Build Cloud Controller & Registry (Linux) - Version 1.4.0 - Oct 18, 2019
- Bug Fix : Reverted "Fixed controller handling of instances failing after start. User reported issue."

### Jenkins Plugin version 1.21.0 - Oct 18, 2019
- New feature : Add a "post build step" to check "save image request" status
- Bug Fix : cache builder - JNLP, Suspend setting doesn't work

### Anka 2.1.2 change - Oct 10, 2019
- Bug Fix : SSH is not turned ON by default on Catalina guests

### Anka Build Cloud Controller & Registry 1.3.1 - Sept 22, 2019
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

### Anka Build Cloud Controller & Registry 1.3.0 - Sept 16, 2019
- Bug Fix : Refreshing any portal page defaults to dashboard page
- New feature : When an instance is in error show the node where it failed in the in portal dahsboard
- New feature : changes required to combine slave template builder and anka Jenkins plugin in one plugin
- New feature : Implement pushing anka agent logs to the controller as part of log reporting
- New feature : Agent shuts down unmanaged running vms when joined to a cluster

### Jenkins Plugin version 1.20.0 - Sept 16, 2019
- New feature : Combine slave template builder and anka Jenkins plugin in one plugin
- Bug Fix : Cancelling Jenkins job doesn't terminate the controller instance request
- Bug Fix : Keep Alive on Error" for a label for Jenkins Pipeline is not working

### Anka Build Cloud Controller & Registry (Linux) - Version 1.2.1
- Bug Fix : Registry has orphan .ank files after executing multiple "reverts"

### Anka Build Cloud Controller & Registry (Mac) - Version 1.2.1
- Bug Fix : Registry has orphan .ank files after executing multiple "reverts"

### Anka Build Cloud Controller & Registry 1.2.0
- Bug Fix : etcd high CPU usage reported by user
- New feature : Automatic upgrade anka agent included in Anka Binary, if there's a newer controller version
- New feature : Controller while spinning VMs on nodes dynamically changes node capacity based on concurrent vCPU/physical cores formula
- New feature : remove -g from ankacluster and add it as default
- New feature : Updated EULA and acknowledgements

### Jenkins Plugin version 1.19.1
- New feature : handle reference to non-existent tag in registry from Jenkins plugin gracefully
- Bug Fix : Support ssh slaves 1.30.0 in jenkins Plugin
- Bug Fix : Jenkins pipeline job doesn't terminate slaves after finishing
- Bug Fix : Plugin not taking slave name field for JNLP

### Jenkins Slave template Builder plug-in version 1.6.0
- Bug Fix : Handle the scenario of special char/whitespace in tag name in slave template prepare plugin
- Bug Fix : Support ssh slaves 1.30.0 in jenkins Plugin
- Bug Fix : subfolder structure in jenkins job causes salve template builder plugin to fail due to special char

### GitLab CI Plugin
- Note : No Changes

See upgrade notes at [here](https://ankadoc.bitbucket.io/#Upgrade Instructions and Notes)

### Version 2.0.1
- Anka Version 2.0.1 - Supports multiple license types for different products. Anka Build Basic, Anka Build Enterprise, Anka Build Enterprise Plus, Anka Flow, Anka Secure
- Anka Build Cloud Controller & Registry combined Linux package - Version 1.1.0
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

### Anka Build Cloud Controller & Registry (Linux) - Version 1.1.0
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

### Jenkins Plugin version 1.19
- New feature : Add Eterprise tier authentication/authorization support in Jenkins Plugin to access Controller
- New feature : Changes to plugin connectivity to controller new configuration parameters

### TeamCity Plugin version 1.5
- New feature : Add Eterprise tier authentication/authorization support in TeamCity Plugin to access Controller