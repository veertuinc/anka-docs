---
---

- (Optional) Automatically join to the Anka Build Cloud Controller using User Data:

  {{< hint warning >}}
  This step requires that you first [set up the Anka Build Cloud]({{< relref "anka-build-cloud/getting-started/setup-controller-and-registry.md#linux--docker" >}}).
  {{< /hint >}}

  {{< hint warning >}}
  **IMPORTANT:** Amazon confirmed that Terminating from the AWS console/API does not properly send SIGTERMs to services and wait for them to stop. This prevents our cloud-connect script from automatically disjoining with `ankacluster disjoin` before AWS pulls the plug. Therefore, we recommend executing the `sudo launchctl unload -w /Library/LaunchDaemons/com.veertu.aws-ec2-mac-amis.cloud-connect.plist` command before termination of the instance.
  {{< /hint >}}

  #### User Data ENVs

  {{< hint info >}}
  For user-data, don't use `;`, `&&` or any other type of separator between envs.
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
  - Only available in > 3.2.0/13.0 AMIs.


#### Manual Preparation

{{< hint warning >}}
By default all of our AMIs have a cloud-connect agent which on boot will join your AWS instance to the Anka Build Cloud controller automatically with [user data ENVs you set]({{< relref "#user-data-envs" >}}). This is issuing `ankacluster join` under the hood. Once joind, the agent which runs and communicates with the Anka Build Controller does its best to determine the proper IP to use for the node. On AWS the interfaces are loaded at different times and orders and often you'll end up with an IP assigned to the node which cannot be used for communication. To solve this, you'll want to set `ANKA_JOIN_ARGS` with `--host {IP HERE}` in the user data for the AWS instance. You can find all available flags/options for the join command [here]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md#joining-to-your-anka-build-cloud-controller" >}}).
{{< /hint >}}

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

{{< hint warning >}}
**Amazon EBS volumes can be very slow even when you max iOPS, etc.** Because of this, `anka create` and other processes can take very long times or outright fail (Apple's installer is sensitive to disk IO). We recommend that you "pre-warm" the EBS volume by running `dd if=/dev/random of=testfile bs=1g count=$(($(df -h | grep "/$" | awk '{print $4}' | grep -oE "[0-9]+")-2))` on the host right after it starts. Additionally, pre-warmed volumes stay warmed -- no need to run `dd` after periods of inactivity on the AWS instance.
{{< /hint >}}

{{< hint info >}}
You can see how we generate these AMIs in our open source repo: https://github.com/veertuinc/aws-ec2-mac-amis.
{{< /hint >}}

#### Logs

- `/var/log/resize-disk.log`
- `/var/log/cloud-connect.log`
