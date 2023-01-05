---
---

{{< hint info >}}
**ARM USERS:** The ipsw files will be downloaded into `img_lib_dir`. You can find the location of this directory with `anka config img_lib_dir`. These (and other temporary) files can be deleted with `anka delete --cache`.
{{< /hint >}}

A few tips when creating VMs:

- We recommend naming your initial VM after the version of macOS.

- **Remember that VM templates are created under a specific user and will not be available to other users.**

- VM performance is important to our users. When setting CPUs for the VMs, 2vCPUs is usually not enough and can cause instability inside of the VM. 
  - **ARM USERS:** VCPUs are determined by taking the physical performance cores and multiplying by 2. This means you can set the CPU on creation for an M1 mini with 4 physical perf cores to `anka create --cpu-count 4`, and run two VMs per host (8vCPUs available).
  - **INTEL USERS:** We recommend taking the total physical cores, doubling it, and then subtracting 2cpu. This leaves you with total virtual cores that can be used by VMs. If you plan to run 2 VMs at a time, divide the total vCPU in half and give the VM the result.

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
**INTEL USERS:** Suspending VMs can sometimes produce a VM which is frozen on start. Usually this is because the hardware & cpu type you created the VM and suspended it on is different from the one you're trying to start it on. Be sure to suspend your VMs on the same hardware that will be running VMs. You can use `ANKA_CREATE_SUSPEND=0 anka create . . .` to produce a stopped VM.
{{< /hint >}}

{{< hint info >}}

**Anka Develop license (default):** While you can create as many VMs as you wish, the free Anka Develop license only allows you to run one VM at a time and will only function on laptops (Macbook, Macbook Pro, and Macbook Air). It only supports a stopped VM state.

{{< rawhtml >}}<br />{{< /rawhtml >}}

**Anka Build license:** When determining how many vcpus and ram your VM needs, you can divide the number of VMs you plan on running simultaneously within a host by the total **virtual cores (vcpus)** it has. So, if I have 12vCPUs on my 6core Mac Mini, and I want to allow 2 running VMs at once and not cripple the host machine, I will set the VM Template/Tag to have 6vcpus (12 / 2). However, with RAM, you'll need to allow ~2GB of memory for the Anka Software and host ((totalRAM / 2)-1). Build licenses support suspended and stopped VM states.

{{< /hint >}}

{{< hint error >}}
**Be aware of the user you're executing Anka CLI commands as.** If you create VMs as root, they won't be available to other users on the system and vice versa.
{{< /hint >}}
