---
---

4. (Optional) Automatically join to the Anka Build Cloud Controller using User Data:
  {{< hint warning >}}
  **IMPORTANT:** Amazon confirmed that Terminating from the AWS console/API does not properly send SIGTERMs to services and wait for them to stop. This prevents our cloud-connect script from automatically disjoining with `ankacluster disjoin` before AWS pulls the plug. Therefore, we recommend executing the `sudo launchctl unload -w /Library/LaunchDaemons/com.veertu.aws-ec2-mac-amis.cloud-connect.plist` command before termination of the instance.
  {{< /hint >}}

    ##### Environment variables you pass in as `user-data`

    {{< hint info >}}
  For user-data, don't use `;`, `&&` or any other type of separator between envs.
    {{< /hint >}}

    {{< hint info >}}
  If you pass in user-data with the exports all on one line, and have non ANKA_ ENVs you're setting, the `cloud-connect.bash` service we run on instance start/boot will source/execute them. We recommend you split exports and user-data onto separate lines to avoid this.
    {{< /hint >}}

    ##### ANKA_CONTROLLER_ADDRESS

    - **REQUIRED**
    - Must be in the following structure: `http[s]://[IP/DOMAIN]:[PORT]`

    ##### ANKA_JOIN_ARGS

    - Optional
    - Allows you to pass in any "Flags" from `ankacluster join --help`

    ##### ANKA_REGISTRY_OVERRIDE_IP + ANKA_REGISTRY_OVERRIDE_DOMAIN

    Allows you to set the registry IP address and domain in the `/etc/hosts` file.

    - Optional
    - Use 1: if your corporate registry doesn't have a public domain name, but does have a public IP
    - Use 2: if you want the EC2 mac mini to pull from a second registry that's hosted on EC2 instead of a local corporate one (AWS -> AWS is much faster)

    ##### ANKA_LICENSE (only available in >= 2.5.4 AMIs)

    If not already licensed, the cloud-connect service will license Anka using this ENV's value.

    - Optional
    - You can also update invalid/expired licenses with this.

    {{< imgwithlink src="images/getting-started/aws-ec2-mac/user-data.png" >}}

#### Manual Preparation

Our AMIs attempt to do the majority of preparation for you, however, there are several steps you need to perform once the instance is started:

1. Set password with `sudo /usr/bin/dscl . -passwd /Users/ec2-user {NEWPASSWORDHERE}`.

2. You now need to VNC in and log into the ec2-user (requirement for Anka to start the hypervisor): `open vnc://ec2-user:{GENERATEDPASSWORD}@{INSTANCEPUBLICIP}`.

{{< hint info >}}
You can see how we generate these AMIs in our open source repo: https://github.com/veertuinc/aws-ec2-mac-amis
{{< /hint >}}

#### Logs

- `/var/log/resize-disk.log`
- `/var/log/cloud-connect.log`
