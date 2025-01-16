---
---

- (Optional) Automatically join to the Anka Build Cloud Controller using User Data:

  {{< hint warning >}}
  This step requires that you first [set up the Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md" >}}) on a Linux server/docker container in AWS (but not on the EC2 Mac instances we provide AMIs for).
  {{< /hint >}}

  {{< hint warning >}}
  **IMPORTANT:** Amazon confirmed that Terminating from the AWS console/API does not properly send SIGTERMs to services and wait for them to stop. This prevents our cloud-connect script from automatically disjoining with `ankacluster disjoin` before AWS pulls the plug. Therefore, we recommend executing the `sudo launchctl unload -w /Library/LaunchDaemons/com.veertu.aws-ec2-mac-amis.cloud-connect.plist` command before termination of the instance.
  {{< /hint >}}

  #### User Data ENVs

  {{< hint info >}}
  For user-data, don't use `;`, `&&` or any other type of separator between envs. You also cannot use multiline strings in the ENVs. However, you can replace newlines with `\n` and our service will get the ENV with the multiple line text
  {{< /hint >}}
  
  {{< hint info >}}
  If you pass in user-data with the exports all on one line, and have non ANKA_ ENVs you're setting, the `cloud-connect.bash` service we run on instance start/boot will source/execute them. We recommend you split exports and user-data onto separate lines to avoid this.
  {{< /hint >}}

  ##### ANKA_CONTROLLER_ADDRESS (string)

  Full URL for the Anka Build Cloud Controller.

  - **REQUIRED**
  - Must be in the following structure: `http[s]://[IP/DOMAIN]:[PORT]`.

  ##### ANKA_JOIN_ARGS (string)

  Allows you to pass in any "Flags" from `ankacluster join --help`.

  - Optional

  ##### ANKA_REGISTRY_OVERRIDE_IP + ANKA_REGISTRY_OVERRIDE_DOMAIN (string)

  Allows you to set the registry IP address and domain in the `/etc/hosts` file.

  - Optional
  - Use 1: if your corporate registry doesn't have a public domain name, but does have a public IP.
  - Use 2: if you want the EC2 mac mini to pull from a second registry that's hosted on EC2 instead of a local corporate one (AWS -> AWS is much faster).

  ##### ANKA_LICENSE (string)

  If not already licensed, the cloud-connect service will license Anka using this ENV's value.

  - Optional
  - Only used with Community AMI.
  - Only available in >= 2.5.4 AMIs.
  - You can also update invalid/expired licenses with this (requires a reboot).
  - Starting in AMIs with a macOS version greater than 12.2.1: The Fulfillment ID output from `anka license activate`, which is used for releasing cores, is logged to your Cloud Controller > Logs section in the "AWS Cloud Connect Service".

  ##### ANKA_USE_PUBLIC_IP (boolean)

  This will determine whether the instance/node is joined using the public ipv4. Otherwise, it will default to the local/private ipv4.
  
  - Optional

  ##### ANKA_CONTROLLER_API_CERT / _KEY / _CA | ANKA_REGISTRY_API_CERT / _KEY / _CA (string)

  The script which handles joining to your controller has a few calls to the controller as well as the registry APIs. If you're protecting your APIs with TLS and Certificate Authentication, you can set the certs to use with these ENVs.

  - Optional

  {{< imgwithlink src="images/getting-started/aws-ec2-mac/user-data.png" >}}


  ##### ANKA_PULL_LATEST_CLOUD_CONNECT (boolean)

  This will issue a `git fetch` and then, if there are changes pending on our [aws-ec2-mac-amis repo](https://github.com/veertuinc/aws-ec2-mac-amis), issue `git pull` to collect the latest version of the Cloud Connect scripts. This is useful if there is a bug in our scripts and you can't update to a newer AMI yet.

  - Optional
  - Only available in 3.2.0/13.0 or greater AMIs.

  ##### ANKA_UPGRADE_CLI_TO_LATEST (boolean)

  This will force an upgrade of Anka Virtualization CLI to its latest version.

  - Optional
  - Only available in 3.3.4/13.4.1 or greater AMIs.
  - This could be dangerous; please don't rely on it unless newer AMIs are not available.

  ##### ANKA_UPGRADE_CLI_TO_VERSION (string)

  This will force an upgrade of Anka Virtualization CLI to the given version. It will perform a `curl -S -L -o ./$FULL_FILE_NAME https://downloads.veertu.com/anka/$FULL_FILE_NAME` so take a look at [https://downloads.veertu.com/#anka/](https://downloads.veertu.com/#anka/) to see a complete list of available packages. You'll need to use the full file name (eg: `Anka-3.6.1.198.pkg`).

  - Optional
  - Only available in 3.6.1/15.1.1 or greater AMIs.

  ##### ANKA_PULL_TEMPLATES_REGEX (string)

  This will pull templates from the registry which match the given regex (egrep) pattern.

  - Optional
  - Only available in 3.3.4/13.4.1 or greater AMIs.
  - Only the latest tag will be pulled.
  - Pulls will happen before the node is joined.
  - If your regex starts with `-`, be sure to escape it with `\-`.

  ##### ANKA_PULL_TEMPLATES_REGEX_DISTRIBUTE (boolean)

  This will distribute, using the controller API, templates that match the given regex (egrep) pattern to the current node. 

  - Optional & Dependent on `ANKA_PULL_TEMPLATES_REGEX`.
  - IMPORTANT: Be sure that your regex will match the template NAME and not the id. Otherwise this will fail.
  - Only available in 3.4.0 or greater AMIs.
  - Only the latest tag will be pulled.
  - Because distribution is asynchronous, the node will be joined in "drained" mode.

  ##### ANKA_DRAINED_ON_JOIN (string)

  This will join the node in [Drain Mode.]({{< relref "whats-new/build-cloud-1.32.0/index.md#drain-mode" >}})

  - Optional
  - Only available in 3.3.10 or greater AMIs.
  - Requires Controller 1.32.0 or greater.

  ##### ANKA_EXECUTE_SCRIPT (string)

  This will execute one of the available scripts included with the cloud-connect service. You must specify the full script name.

  - [A list of available scripts is available here.](https://github.com/veertuinc/aws-ec2-mac-amis/tree/main/scripts)
  - Optional
  - Only available in Anka 3.4.0 or greater AMIs.
  - If the script fails, the node will still join.
  - Most scripts support ENVs being passed in through user-data, so be sure to review them to see what's possible. Dev note: the cloud-connect will not set/see any ENVs without ANKA_ prefix.

#### Manual Preparation (optional)

**Amazon EBS volumes can be very slow even when you max iOPS, etc.** Because of this, `anka create` and other processes can take very long times or outright fail (Apple's installer is sensitive to disk IO). AWS indicates that you have to pre-warm EBS volumes that are restored from snapshots (which our AMIs are). To do this, [follow the instructions outlined here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-initialize.html#ebs-initialize-linux): `brew install fio && sudo fio --filename=/dev/r$(df -h / | grep -o 'disk[0-9]') --rw=read --bs=1M --iodepth=32 --ioengine=posixaio --direct=1 --name=volume-initialize` Finally, pre-warmed volumes stay warmed -- no need to run `dd` after periods of inactivity on the AWS instance. **NOTE: This command is not able to run from user-data.**

{{< hint warning >}}
By default all of our AMIs have a cloud-connect agent which on boot will join your AWS instance to the Anka Build Cloud controller automatically with [user data ENVs you set]({{< relref "#user-data-envs" >}}). This is issuing `ankacluster join` under the hood. Once joined, the agent which runs and communicates with the Anka Build Controller does its best to determine the proper IP to use for the node. On AWS the interfaces are loaded at different times and orders and often you'll end up with an IP assigned to the node which cannot be used for communication. To solve this, you'll want to set `ANKA_JOIN_ARGS` with `--host {IP HERE}` in the user data for the AWS instance. You can find all available flags/options for the join command [here]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}).
{{< /hint >}}

<!-- 
Our AMIs attempt to do the majority of preparation for you, however, there are several steps you need to perform once the instance is started:

1. Enable VNC:

```bash
sudo defaults write /var/db/launchd.db/com.apple.launchd/overrides.plist com.apple.screensharing -dict Disabled -bool false
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.screensharing.plist
```

1. Set password with `sudo /usr/bin/dscl . -passwd /Users/ec2-user {PASSWORD HERE}`

{{< hint info >}}
Some of our older AMIs (2.5.7 or older) set a default password to `zbun0ok=`. We no longer do that in AMIs by default for security reasons. **It is unsafe to continue to use the default password we set.** You can change it with `sudo /usr/bin/dscl . -passwd /Users/ec2-user zbun0ok= {NEW PASSWORD HERE}`
{{< /hint >}}

1. You now need to VNC in and log into the ec2-user (requirement for Anka to start the hypervisor): `open vnc://ec2-user:{NEWPASSWORDHERE}@{INSTANCEPUBLICIP}`.


{{< hint info >}}
You can see how we generate these AMIs in our open source repo: https://github.com/veertuinc/aws-ec2-mac-amis.
{{< /hint >}} -->

#### Logs

- `/var/log/resize-disk.log`
- `/var/log/cloud-connect.log`
