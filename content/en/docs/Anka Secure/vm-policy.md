


---
title: "VM Policy"
linkTitle: "VM Policy"
weight: 4
date: 2019-12-12
description: >
  VM Policy is extensible and flexible mechanism to control access to certain resources and functionality while at the same time preserving consistency and hardening runtime for increased security.
---




## Creating VM Policy

Core elements of the VM Policy are:  

1. Users (and groups)
2. Rules  

Every vm policy can be treated as a user database (accounts, participating in policy). Every user, registered in the policy belongs to groups, group has similar parameters as a user, for example name and associated rules. Unlike the users, groups doesn’t contain authorization and crypto information, so group name can’t be used to access the policy.  

Group can be a part of another group, so the policy is actually tree structure of users and groups.  

The “standard” group is the root of the tree, every user and group is part of this group. Another predefined group is “administrators”. Administrators is part of the standard group, but has some fundamental differences:  

1. Only administrators can modify policy.
2. Policy has to have at least one administrator.

### Rules
Another part of the VM Policy is rule. Rule is a literal string rulename with assigned verdict:TRUE or FALSE.  

rulename=verdict  

Example - `com.veertu.anka.net.local=disable`  

Rule is not being used by policy itself, conversely systems (VM in our case), using the policy, associate their internal functionality with certain rulename.  

Examples:  

`com.veertu.ankahv.fs` controls shared folder functionality for the VM.  
 
`com.veertu.ankahv.net` controls VM networking.




Behavior of the VM can be altered by changing the VM policy.  

The rulename has reverse domain name structure (dot separated), and processing of rule is also happening in direction from larger scope to more specific one. Rule that describes more generic scope affects all subscopes, and “sub rules” could override rules of larger scope.  

`com.veertu.anka.fs=deny` - specified in policy, disables ability to mount any host folder into VM.  

Additional “sub rule" `com.veertu.anka.fs./Users/test/workspace1=allow` overrides more generic com.veertu.anka.fs rule, and allows to mount workspace1 (and any of its subfolders) of system user test.  

Policy rules are used to control entire VM functionality: ability to start, modify, access network, use shared paste buffers, external drives, USB, etc. Rules mechanism is pretty flexible, so it’s easy to add additional controls in the future.

Anka VM policy rules can have three kinds of “verdicts”.  

1. Logical TRUE (allow, enable, 1, yes, on, pass, ...)
2. Logical FALSE (deny, disable, 0, no, off, block, ...)
3. Undefined (the rulename is not specified in the policy)

By default all runtime features, like networking, shared folders, are “enabled”, so to disable them it’s needed to explicitly specify corresponding denial rule in the policy.

Contrary to this, all configuration changes - changing RAM size, VNC password, etc (except some needed for normal operation of anka tool) are prohibited by default to non-administrators.

To allow such changes, explicitly specify corresponding rule in the policy.  

Runtime functionality rules have the following form - `com.veertu.ankahv.{function}={verdict}`

Rules for VM configuration have the following form - `com.veertu.anka.cfg.{parameter}={verdict}`

### Anka Secure policy Management  

VM Policy can be modified by users with a private key. Administrators have such key, while regular users have only public key, so they are only able to read policy. Groups have no keys at all, so they can’t be used to open policy file.  

This design prevents modification of policy files by non-admin users. It’s safe to share policy files over unencrypted channels and store in plain files - no additional protection is required. But such design also introduces some limitations in assigning user’s passwords - only administrator can modify user’s data, the user is not able to change their password in policy by themselves.  

To get temporary passwords, password renewal (by user) and other “privacy” related things, additional external mechanisms should be invoked.  

Following algorithms are used - RSA-2048, SHA-256, AES-256 . 

Critical policy parts are encrypted with user’s password (aes-cbc-256).  

**Policy Management Tool**  

Current implementation of VM Policy uses separate json files to store user/rules db and cryptographic information. To maintain policy and sign VM configuration files the policy tool is used. This tool is being shipped with Anka package 2.0 , available with Anka Secure license and above (located at `/Library/Application\ Support/Veertu/Anka/bin/policy`).  

This tool can be compiled and used under Linux and (probably) other posix compliant systems.
```
/Library/Application\ Support/Veertu/Anka/bin/policy
usage: policy [options] <command>

   Manipulate Anka policy (create, add, assign, sign, ...)

options:
  -l,--login <val>   Username to access policy file
  --version          Print version information and quit
  --help             Show usage for particular command and exit

commands:
  create             Create new policy file with initial admin user
  adduser            Add new user to policy
  addgroup           Add new group to policy
  delete             Delete user or group from policy
  join               Add user/group to certain group
  leave              Remove user from group(s)
  users              List the policy users
  groups             List the policy groups
  assign             Assign rule(s) to certain user or group
  retract            Retract rule(s) from certain user or group
  rules              List rules applied to the user or group
  decide             Evaluate the rule for the user returning corresponding status
  sign               Sign input (yaml) file
  validate           Validate integrity of (yaml) file
```
Every policy has to have at least one administrator user. This user is created during the creation of new policy file. Later users and groups can be added with corresponding commands.  

The policy tool infers “current” user from command line option, environment, and current macOS user name in the following order.  

1. `--login <username>` command line option
2. `ANKA_USER` environment variable,
3. current system user name (a user under which the policy tool was started)

Some commands don’t require authentication and could be invoked anonymously, like users, groups & rules.  

Password is required to authorize certain user. The password could be provided through ANKA_PASSWORD environment variable, or asked interactively through tty (if connected). If no tty is connected to the policy process and the ANKA_PASSWORD variable is not found, the user's password is assumed to be EMPTY (that is also valid, but insecure, password).  

**Turn Policy on for a VM** - `anka modify VM set policy`  

This command creates policy.json file inside VM folder with initial admin user, inferred from arguments and environment as discussed above. Initial policy has some predefined minimal set of rules to allow core Anka logic (i.e. clone, suspend, push, pull etc) to operate.  

**View active rules (for current user)** - `policy rules /path/to/vm_dir/UUID/policy.json`  

This operation doesn’t require actual authorization.  

VM Policy can be changed later (with the policy tool) to allow non-administrators to make some changes like change cpu count, RAM size, configure display settings etc.  

**Disable VM Policy** - `anka modify VM delete policy`  

Only administrators can delete the VM policy.  

**Clone** - Clone operation preserves current policy settings and so it’s allowed to all users by default. `clone --copy` produces pure (no policy) VM. So `clone --copy` operation can be performed only by the VM administrator.  

**Push and Pull** - Pushing and pulling of VM is also allowed to all users by default. Implement certificate based access to the registry is supported to control access for the users.

***Note*** VM functions that don’t affect VM configuration and runtime directly (anka config, anka registry,cluster etc) are not controlled by the policy.

### Anka Secure Policy Rulename List

`com.veertu.ankahv.start=0` To disable VM to start by authorized user (non-authorized ones can’t start in any case)


**Networking**
`com.veertu.ankahv.net.local=0` disable "local" (interhost) connectivity
`com.veertu.ankahv.net.ip=0` set all IPs blacklisted by default
`com.veertu.ankahv.net.ip.{ip.ad.dre.ss}=1` whitelist specific outgoing IP address
`com.veertu.ankahv.net.tcp=0` disable TCP protocol
`com.veertu.ankahv.net.udp=0` disable UDP protocol
`com.veertu.ankahv.net.other=0` disable other (that TCP and UDP)

Special `com.veertu.ankahv.local=0` rule disables any connectivity (Ethernet and IP levels) among host where VM is running. This includes

1. Inter vm connectivity
2. VM - Host connectivity
3. VM - host bridge and aliases connectivity
4. Zero configuration protocols (DNS, DHCP, ARP..) handling  

***Note*** The user can still build custom network topology, e.g. connect personal laptop with patch cord, and perform uncontrollable (from the perspective of corporate network) data transfers.  

**Protocols**

`com.veertu.ankahv.net.ether.{prot_number}=1` allow specific network protocol (ethernet)
`com.veertu.ankahv.net.fwd=0` disable all port forwarding rules
`com.veertu.ankahv.net.fwd.{dport}.{addr}.{sport}=1` allow specific port forwarding rule. {lp} is port opened on the host, {addr} is a listening address, for example 0.0.0.0 receives connections from any peer, {dp} is service port in VM.  

Due to reverse domain name essense of the rulename processing, there could be some “wildcards” in the port forwarding rules.  

`com.veertu.ankahv.net.fwd.20000.192.168.10.1=1` - Allows connections to any port inside VM from host 192.168.10.1 through port 20000  

**Disk Access**
`com.veertu.ankahv.disk=0` prevent access to any unencrypted drives
`com.veertu.ankahv.disk.%s=1` allow access to certain unencrypted drive

**Shared Folders and Anka Run**
`com.veertu.ankahv.fs=0` disable ability to mount host folders
`com.veertu.ankahv.fs.{/abs/path/to}=1` allow to mount specific host folder
`com.veertu.ankahv.run=0` disable anka run functionality
`com.veertu.ankahv.run.{/abs/path/to}=1` allow to run certain commands via anka run

**Anka View**
`com.veertu.ankahv.view.pb=0` disable shared paste bufferd(PasteBoard) in Anka View
`com.veertu.ankahv.view.hid=0` disable keyboard pass-through in Anka View

**Anka VNC**  
Anka VM has it's own implementation of VNC (RFB) server, allowing to access guest’s display via the network protocol. Additionally macOS guests have Screen Sharing service, also allowing to interact with the desktop over network.  

Anka VNC doesn’t allow to use any kind of paste buffer sharing, moreover once the policy is enabled for certain VM, the VNC password became encrypted.  

```
display:
frame_buffer:
height: 768
password:
X5NUmK5SkspAyQ5277yqZK1p6qk8xYx+JNuhS8PQRYPa6ejLh80IlbOX4I+qcmrTTzSjZ
y7Jfj4WqlTxonZmV3FGNk2g5+C9INPx0nLF4GnddWlP3zXbIDgMDBsELWQQK6f69VPkuU
akyIBQxTHLANOGV+WjujVvI4jygwZwoVZICVvYzygO2g0mDFzD2Dl9RO1S9Zowtna1su4
D/MyQTJz7cBsRBO9xw1uJb/uLYTS5TqlGOPjU4ksXhsvfd1Tko2qPVhFjG7Pf7L1gPYv0
3813eCq9mKVVx/A5tUuCVIObCfjiodAI+1J8HsJ6/nfPlW9ZurYYkAUCVHSU9e/SzA==
pci_slot: 29
vnc_ip: 0.0.0.0
width: 1024
```
Users who don’t know Anka VNC password are not able to infer it neither with [`anka describe`]({{< relref "docs/Anka CLI/command-reference.md#describe" >}}), neither [`anka show`]({{< relref "docs/Anka CLI/command-reference.md#show" >}}) commands. There is no special policy rule, controlling Anka VNC module for now.  

To control connectivity to the guest’s Screen Sharing service, disable local networking, and close guest port 5900 from port forwarding.

`com.veertu.ankahv.net.local=0`
`com.veertu.ankahv.net.fwd.5900=0`

**USB**  

USB Pass-Through is provided by the uhost module. Policy allows to prevent this module from loading. So with policy USB pass-through could be enabled or disabled only generally - on all kinds of devices.  

`com.veertu.ankahv.usb.{module}=0` disable corresponding USB Module (“host”)