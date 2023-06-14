---
title: "VM indicating wrong architecture (ARM)"
weight: 1
---

## Scenario

ARM users may notice that commands like `arch` indicate the VM is i386/intel. Or, `system_profiler SPHardwareDataType` `chip_type` will be Unknown:

```
{
  "SPHardwareDataType" : [
    {
      "_name" : "hardware_overview",
      "activation_lock_status" : "activation_lock_disabled",
      "boot_rom_version" : "8419.80.7",
      "chip_type" : "Unknown",
      "machine_model" : "VirtualMac2,1",
      "machine_name" : "Mac",
      "model_number" : "VM0001LL/A",
      "number_processors" : 6,
      "os_loader_version" : "8419.80.7",
      "physical_memory" : "14 GB",
      "platform_UUID" : "E465E3CB-0162-5E0E-9CEA-FED57C72BE74",
      "provisioning_UDID" : "E465E3CB-0162-5E0E-9CEA-FED57C72BE74",
      "serial_number" : "ZNNF3941VQ"
    }
  ]
}
```

## Solution

This is caused by running an amd64 process or service on the VM. Rosetta will start Translating all other operations through Rosetta. You'll want to ensure all software installed inside of the VM was built for `arm64`.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

