---
---

{{< hint info >}}

- We recommend naming your initial VM after the version of macOS.

- **Remember that VM templates are created under a specific user and will not be available to other users.**

- By default, 2vCPUs is not enough and will cause instability inside of the VM. VCPUs are determined by taking the physical performance cores and multiplying by 2. This means you can set the CPU on creation for an M1 mini with 4 physical perf cores to `anka create --cpu-count 4`, and run two VMs per host (8vCPUs available).
{{< /hint >}}
