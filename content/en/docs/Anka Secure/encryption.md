


---
title: "VM Disk Encryption"
linkTitle: "VM Disk Encryption"
weight: 5
date: 2019-12-12
description: >
  Disk encryption logic plays an important role in VM Policy infrastructure. 
---


Anka Disk Engine is closely integrated with VM Policy and performs on the fly encryption (OTFE) of all information passed to/from disk image. Encryption uses cryptography data from the associated policy, so it’s not possible to use (decode) encrypted drive with another policy or without policy itself. This robustly protects encrypted VMs from policy replacement attacks.  

OTFE is not turned on by default with `anka modify VM set policy` command. It can be turned on with `anka modify VM set hard-drive` command.  

```
anka modify Mojave10145 set hard-drive --help
Usage: anka modify set hard-drive [OPTIONS] INDEX

  modify hard drive settings

Options:
  -s, --size TEXT                 Disk size
  -e, --enc [aes-128|aes-192|aes-256|none]
                                  Set OTFE algorithm
  --help                          Show this message and exit.
```

Currently aes-ecb encryption algorithm is supported. 128 bit encryption is more than enough to protect VM from unsolicited access/modifications, but for some applications aes-ecb-256 could be needed.  

