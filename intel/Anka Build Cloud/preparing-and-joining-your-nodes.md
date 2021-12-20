---
date: 2019-12-12T00:00:00-00:00
title: "Preparing and Joining your Nodes to the Controller"
weight: 4
description: >
  How to join your Anka Build Virtualization Nodes to the Anka Build Cloud Controller
---

## Preparing your Nodes

Once the Anka Build Virtualization software has been installed onto a macOS machine, you'll want to turn off sleep and several other default features that would cause the machine to become unavailable.

{{< hint warning >}}
This guide has a few options that are typically not available on MDM controlled machines. You may need to ask your system administrators/local IT team to allow them.
{{< /hint >}}

{{< hint warning >}}
Be sure to reboot the host after applying these changes.
{{< /hint >}}

### Required

- **Start a UI session by logging into your node's user** to ensure Apple's services are started (it doesn't have to be an administrator).

- **Enable `Automatic Login` for the current user:** Go to Preferences > Users > Enable Automatic Login for the current user. Or, [using the CLI](https://github.com/veertuinc/kcpassword).
  {{< hint warning >}}
  Not available if FileVault is enabled or you use an iCloud password to log into the user.
  {{< /hint >}}
  {{< hint info >}}
  If using the CLI method of enabling `Automatic Login`, you must also XOR-encrypt the login password and add it to `/etc/kcpassword`.

  The `/etc/kcpassword` file must be owned by `root:wheel` with a mode of `0600`.

  See the GitHub repository [veertuinc/kcpassword](https://github.com/veertuinc/kcpassword) for help generating the encrypted password string.
  {{< /hint >}}

- **Uncheck `Log out after X minutes of inactivity`** under System Preferences > Security & Privacy > Advanced

- **Uncheck `Require password X after sleep or screen saver begins`** under System Preferences > Security

  {{< hint warning >}}
  VNC may be required to disable this. It's possible your hardware does not have VNC enabled and you also don't have physical access. The following are the commands necessary to enable VNC from an SSH:
```bash
The following examples are only for OS X 10.5 and later.
cd /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/
sudo ./kickstart -configure -allowAccessFor -specifiedUsers
sudo ./kickstart -configure -allowAccessFor -allUsers -privs -all
sudo ./kickstart -activate
```
  {{< /hint >}}

- **Disable all forms of sleep:** Go to Preferences > Energy Saver > disable any sleep options.

  {{< hint info >}}
  Alternatively, you can do this from the command-line:

```shell
# Never go into computer sleep mode
sudo systemsetup -setcomputersleep Off
systemsetup -setcomputersleep Off || true
# Disable standby
sudo pmset -a standby 0
# Disable disk sleep
sudo pmset -a disksleep 0
# Hibernate mode is a problem on some mac minis; best to just disable
sudo pmset -a hibernatemode 0
```
  {{< /hint >}}

- **Run `sudo anka create test && sudo anka delete --yes test` at least once for the user you plan on running VMs as.**

### Optional but recommended

- If on macOS version Big Sur: Disable Apple's mitigations with `sudo anka config vmx_mitigations 0`. Without it, performance will be ~10% worse inside of the VM. (This does not work on Monterey versions of macOS)

- Disable spotlight:

    If you cannot perform launchctl commands, you can execute these commands from the command-line:

    ```shell
    # Disable indexing volumes
    sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Volumes"
    sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array "/Network"
    sudo killall mds
    sleep 60
    # Make sure indexing is DISABLED for the main volume
    sudo mdutil -a -i off /
    sudo mdutil -a -i off
    ```

    MDS can be disable with `sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist`, but only if SIP is disabled on the host (not recommended)

- Set the same `chunk_size` across your nodes with `anka config chunk_size 2147483648` (2GB; multiple of `1048576`). Any of the machines that create VM templates/tags need to also have this set.

  {{< hint warning >}}
  If the chunk_size is too small, you may hit `Too many open files` when trying to start VMs. You can try to modify the system's maxfiles (`sudo launchctl limit maxfiles 4096 unlimited`), but it may also be a good idea to increase the `chunk_size` to a larger size as well.
  {{< /hint >}}

{{< hint info >}}
You may also want to have your nodes restart on host level failure: `systemsetup -setrestartpowerfailure on` & `systemsetup -setrestartfreeze on`
{{< /hint >}}


## Joining Prerequisites

- [Anka Build Cloud Controller should be configured and running before your Anka Node can join.]({{< relref "intel/Anka Build Cloud/_index.md" >}})

- [Your Node should be licensed.]({{< relref "intel/Licensing/_index.md" >}})

- [Your Node should be prepared for high availability.]({{< relref "#preparing-your-nodes" >}})

## Joining to your Anka Build Cloud Controller

{{< hint warning >}}
Be sure to run ankacluster as root.
{{< /hint >}}

{{< hint warning >}}
Avoid using underscores in your domainnames/urls.
{{< /hint >}}


```shell
❯ sudo ankacluster join http://anka.controller
Testing connection to the controller...: Ok
Testing connection to the registry...: Ok
Success!
Anka Cloud Cluster join success
```

> You can join a Node to multiple controllers by comma separating them:
> `sudo ankacluster join http://anka.controller1,http://anka.controller2`

```shell
❯ ankacluster join --help
Joins the current machine to one or many Anka Build Cloud Controllers

Usage:
  ankacluster join CONTROLLER_ADDRESS[,CONTROLLER_ADDRESS2] [flags]

Flags:
      --api-key-id string           API Key identifier
      --api-key-string string       API Key string
  -c, --cacert string               Specify the path to your Root CA Certificate (PEM/X509)
  -M, --capacity-mode string        Set the capacity mode (resource or number) the Node will use when pulling jobs from the Anka Cloud Cluster queue. 'resource' will look at available resources (see --vcp-override and --ram-override) / 'number' will only accept if --max-vm-count isn't already met (default "number")
  -C, --cert string                 Specify the path to the Node's certificate file (PEM/X509)
  -K, --cert-key string             Specify the path to the Node's certificate key file (PEM/X509)
      --enable-vm-monitor           Enabled unresponsive VM monitoring. This will throw a failure when the VM becomes unresponsive for longer than the --vm-stuck-timeout
  -f, --force-no-sudo               Force the anka_agent to start without sudo
  -g, --global                      DEPRECATED! Install agent into system domain
  -G, --groups string               Specify group name (or multiple names sepearated by ',') to add the current Node to
      --heartbeat duration          Set the duration between status updates the Node sends to the Anka Cloud Cluster (default: 5 seconds)
  -h, --help                        help for join
  -H, --host string                 Set the address (IP or Hostname) of the Node that the Anka Cloud Cluster will use when communicating with CI tools/plugins. This is useful when your CI tool cannot connect to the Node's local IP address (the default value of --host), but does have access to an external IP or hostname for it (proxy, load balancer, etc).
  -k, --keystore string             Specify the path to your certificate keystore (PEM/PKCS12)
  -p, --keystore-pass string        Specify the password for your certificate keystore
  -m, --max-vm-count int            Set the maximum number of VMs this Node is allowed to run (default: 2) (default 2)
  -n, --name string                 Set a custom Node name (default: hostname)
      --no-central-logging          Disable sending logs to central logging location
      --node-id string              Custom node id
  -R, --ram-override int            Set the the max RAM (in GB) that this Node can handle (default: {total ram} - 2GB)
      --reserve-space string        Disk space to reserve when pulling. Number followed by magnitude (1024B, 10KB, 140MB, 45GB...) (default: 20% of disk size)
  -r, --root-cert string            Identical to --cacert
      --skip-tests                  Disable testing the connection before starting the agent
      --skip-tls-verification       Skip TLS verification
  -t, --tls                         Enable TLS for communicating with the Anka Cloud Cluster
  -V, --vcpu-override int           Set the max vcpus that this Node can handle. (default: {current physical cpu count} * 2)
      --vm-stuck-delay duration     The time between unresponsive VM checks (default: 30s - Duration examples: 3500s, 20m, 5h)
      --vm-stuck-timeout duration   The time to wait until the VM is considered unresponsive (default: 10s - Duration examples: 3500s, 20m, 5h)
  ```

{{< hint info >}}
The Anka agent is listening on a socket to provide status information at runtime.
You can override the path of the socket by setting the `ANKA_AGENT_SOCKET` env var.
{{< /hint >}}

## Disjoining

{{< hint info >}}
You don't need to disjoin nodes to upgrade the Anka Virtualization package.
{{< /hint >}}

```shell
❯ sudo ankacluster disjoin
Disjoined from the Anka Cloud Cluster
```

## Check Join Status

You can check the status of the Anka agent using the `ankacluster status` command.


```shell
❯ ankacluster status
status: running
config:
  vm_limit: 2
  optimization_threshold: 5
  num_workers: 2
  controller_addresses:
  - http://anka.controller.dev
  version: 1.13.0-6cd34a2c
  capacity_mode: number
  heartbeat: 5s
  node_name: MyMacMiniNode
  vm_stuck_check_delay: 30s
  vm_stuck_check_timeout: 10s
```

## Answers to Frequently Asked Questions

- Errors like the following are typically the cause of an older version of the agent on your node. You'll want to visit http://downloads.veertu.com, download the agent version matching your controller's version, and then try your ankacluster join command again:
    ```bash
    > sudo ankacluster join http://anka.controller --reserve-space 20GB
    Testing connection to controller...: Ok
    Testing connection to the registry...: Ok
    Ok
    exit status 2
    Log file created at: 2021/07/16 12:36:10
    Running on machine: hostname1
    Binary: Built with gc go1.14.3 for darwin/amd64
    Log line format: [IWEF]mmdd hh:mm:ss.uuuuuu threadid file:line] msg
    I0716 12:36:10.164826  8172 log_cleaner.go:34] clean /var/log/veertu/anka_agent*...
    I0716 12:36:10.166385  8172 log_cleaner.go:56] don't touch: /var/log/veertu/anka_agent.hostname1.root.log.INFO.20210716-123610.8172
    I0716 12:36:10.172275  8172 main.go:173] starting ankaAgent
    I0716 12:36:10.172282  8172 main.go:174] Anka Agent version: 1.7.1-9545c9f5
    I0716 12:36:10.965625  8172 runner.go:43] Anka version is 2.4.1
    I0716 12:36:10.965806  8172 runner.go:46] Agent Version is v2
    Joining cluster failed
    ```
- The Controller will be checking disk space on every pull/preparation of a VM. If not enough disk space is available, it will automatically delete VM Templates/tags from the Node based on **which has the oldest last used timestamp** until there is enough space for the VM Template/Tag. This cannot be disabled at the moment. This can be modified to some extent by using --reserve-space flag (see explanation above)
- The registry `External URL` (seen in the controller UI/config) is used by the Nodes to pull down templates and tags. Be sure to set this URL properly in your Build Cloud Configuration and ensure firewalls allow communication.
