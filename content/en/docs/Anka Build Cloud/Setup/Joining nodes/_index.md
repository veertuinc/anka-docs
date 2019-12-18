
---
date: 2019-12-12T00:00:00-00:00
title: "Joining/Disjoining Anka nodes to Anka Build Cloud using Ankacluster CLI"
linkTitle: "Joining/Disjoining Nodes"
weight: 3
description: >
  Anka Agents (Mac running Anka.PKG) are called Anka Nodes. Multiple nodes can be attached to the Controller to configure Anka Build Cloud..
---

[//]: # (TODO: split this into files)


### Joining Anka Agent

***Pre-Requisites*** - Anka Controller should be configured and running before an Anka Node can be joined.

***Note*** Make sure Autologin and neversleep option are set on Anka mac nodes. 

This step adds the mac hosts running Anka Agent package to the Anka Build cloud Controller.  

Execute this command on the mac hosts running Anka package with Build license - `sudo ankacluster join <http://controllerIP:port>.  
`
You can also set other parameters like name(the name that shows up in controller dahsboard for this host), max-vm-count (number of concurrent VMs to run) etc. See the full list of flags available with this command.
 
When joining the nodes using `ankacluster join`, use sudo. Anka is multi user environment like unix. When you join with sudo, controller will start VMs with sudo. You can view them with `sudo anka list` and operate with these VMs using sudo with other anka commands.  

***Note*** - Starting from version 1.2.0, `ankacluster` command will now ask to be run with sudo. So `ankacluster join http://<host>` will return available flags and a error message. It’s possible to override this behaviour with the `-f` or the `--force-no-sudo` flag.


```
Usage:
  sudo ankacluster join [controller_address] [flags]

Flags:
  -c, --cacert PATH              PATH to CA bundle file (PEM/X509) (optional)
  -M, --capacity-mode string     Capacity mode (resource|number). 'resource' will take vm jobs according to resources, number will take vm jobs according to max-vm-count (default "number")
  -C, --cert PATH                PATH client certificate file (PEM/X509) (optional)
  -K, --cert-key PATH            PATH client certificate key file (PEM/X509) (optional)
  -f, --force-no-sudo            Force the agent to start without sudo
  -g, --global                   DEPRECATED! Install agent into system domain (optional)
  -G, --groups string            Specify group name (or names sepearated by ',') to add this node to (optional)
  -h, --help                     help for join
  -H, --host string              Specify host name or IP for this machine (optional)
  -k, --keystore PATH            PATH to certificate and keystore (PEM, PKCS12) (optional)
  -p, --keystore-pass PASSWORD   PASSWORD for certificate and keystore (optional)
  -m, --max-vm-count NUMBER      Maximum NUMBER of VMs this node is allowed to run (optional) (default 2)
  -n, --name NAME                Node NAME alias (optional)
      --no-central-logging       Dont send logs to central logging (optional)
  -R, --ram-override int         Override the max RAM in GB that this node can handle. defaults to (total ram - 2GB)
  -r, --root-cert PATH           PATH to CA bundle file (PEM/X509) (optional)
      --skip-tls-verification    Skip TLS verification (optional)
  -t, --tls                      Use tls for communicating with the queue (optional)
  -V, --vcpu-override int        Override the max vcpu that this node can handle. defaults to (cpu count * 2)
```
***Note*** - New in version 1.2.0 - Control the concurrent VM count execution on a node using a static number of dynamic capacity available. For dynamic capacity, use the `-M, --capacity-mode string` in conjuction with `-m, --max-vm-count NUMBER` and `-R, --ram-override int` flags. Default is based on static Number and is 2.  

***Note*** - In version 1.2.0, `-g` flag is deprecated but backward compatibility is supported.

***Note*** - In version 1.3, controller will auto shutdown any running unmanaged VMs under sudo anka list. For any manual maintainence of VMs under sudo, disjoin the node first.

***Note*** - In version 1.3, the anka agent running on the nodes sends logs to the central controller. This is useful for debugging purposes. This can be disabled with --no-central-logging during ankacluster join command.

### Disjoining Anka Agent
This step will remove the node from the Anka Build cloud. Execute this when you want to do maintainence on the node etc. You don't need to disjoin nodes to upgrade Anka on the node.  

```
sudo ankacluster disjoin
```
***Note*** - In version 1.2.0, `-g` flag is deprecated for the `disjoin` command.

## Anka Agent Upgrade

### Upgrading Anka Package
1. Before upgrading, you will also need to force stop any suspended VMs with the `anka stop -f vmname` command.
2. Download the latest version of Anka package (Anka.pkg).
3. Install the new package.

***Note*** For major Anka releases, it maybe required to upgrade guest addons in existing Anka VMs. Check the release notes to identify if this step is required or not.

4. Upgrade guest addons on the VM. Execute the `anka start -u vmname` to upgrade guest addons in existing VMs to the current release. If you are not able to upgrade the guest add-ons tool using the `anka start -u vmname` command, then you have a very old version of guest addon tools on your VM. You will first need to manually update them. Contact Veertu support.
5. Push your upgraded VMs to the Registry.

```
anka stop -f vmname
anka start -u vmname
Preparing update configuration
Installing updates
Suspending
update succeeded
```
