---
---

{{< hint info >}}
**ARM USERS:** The ipsw files will be downloaded into `img_lib_dir`. You can find the location of this directory with `anka config img_lib_dir`. These (and other temporary) files can be deleted with `anka delete --cache`.
{{< /hint >}}

A few tips when creating VMs:

- We recommend naming your initial VM after the version of macOS.

- **Remember that VM templates are created under a specific user and will not be available to other users.**

- VM performance is important to our users. When setting CPUs for the VMs, 2 CPUs is usually not enough and can cause instability inside of the VM. Please see [Modifying Your VM]({{< relref "anka-virtualization-cli/getting-started/modifying-your-vm.md#arm" >}}) for more information.

- If you experience issues, run `anka --debug create. . .` and provide it to Veertu's support.

- You can re-enable SIP on intel VMs with `anka modify {vmNameOrUUID} set custom-variable sys.csr-active-config 0` post-create.

{{< hint info >}}
RAM, DISK, and CPU are all set from the defaults under the Anka configuration:
  ```bash
  ❯ anka config | grep default
  | default_disk                | 137438953472                                                                      |
  | default_nvcpu               | 4                                                                                 |
  | default_ram                 | 4294967296                                                                        |
  ```
{{< /hint >}}

{{< hint warning >}}
**INTEL USERS:** Suspending VMs can sometimes produce a VM which is frozen on start. Usually this is because the hardware & cpu type you created the VM and suspended it on is different from the one you're trying to start it on. Be sure to suspend your VMs on the same hardware that will be running VMs.
{{< /hint >}}

{{< hint info >}}

**Anka Develop license (default):** While you can create as many VMs as you wish, the free Anka Develop license only allows you to run one VM at a time and will only function on laptops (Macbook, Macbook Pro, and Macbook Air). It only supports a stopped VM state.

{{< /hint >}}

{{< hint error >}}
**Be aware of the user you're executing Anka CLI commands as.** If you create VMs as root, they won't be available to other users on the system and vice versa.
{{< /hint >}}
