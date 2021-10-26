


---
title: "VM Disk Encryption"
linkTitle: "VM Disk Encryption"
weight: 5
date: 2019-12-12
description: >
  Disk encryption logic plays an important role in VM Policy infrastructure. 
---


Anka Disk Engine is closely integrated with VM Policy and performs on the fly encryption (OTFE) of all information passed to/from disk image. Encryption uses cryptography data from the associated policy, so it’s not possible to use (decode) encrypted drive with another policy or without policy itself. This robustly protects encrypted VMs from policy replacement attacks.  

OTFE is not turned on by default. You can enable it with:

```bash
anka -l {policyAdmin} modify {VM} set policy
[ENTER PASSWORD FOR {policyAdmin}]
anka modify {VM} set hard-drive 0 --enc aes-128 ## This merges all layers together into one, so you lose any VM template layer sharing/disk optimization ##
anka -l {policyAdmin} start vm
```

This start will prompt for the `{policyAdmin}` password you set in the set policy command. To avoid the need to put in the password, you can set `ANKA_USER={policyAdmin}` and `ANKA_PASSWORD={policyAdminPasswordHere}` on the machine.

Once your VM is started, the `anka run` commands work just fine without any `-l` or password/envs. Enabling policy forces any start/stop/modify commands to require the policy admin and password, else it will throw an error.

Currently aes-ecb encryption algorithm is supported. 128 bit encryption is more than enough to protect VM from unsolicited access/modifications, but for some applications aes-ecb-256 could be needed.  

