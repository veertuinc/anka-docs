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

{{< hint info >}}
RAM, DISK, and CPU are all set from the defaults under the Anka configuration:
  ```bash
  ❯ anka config | grep default
  | default_disk                | 137438953472                                                                      |
  | default_nvcpu               | 4                                                                                 |
  | default_ram                 | 4294967296                                                                        |
  ```
{{< /hint >}}
