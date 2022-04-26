---
---

{{< hint error >}}
**Be aware of the user you're executing Anka CLI commands as.** If you create VMs as root, they won't be available to other users on the system and vice versa.
{{< /hint >}}

- We recommend naming your initial VM after the version of macOS.
- The VM creation should take around 40 minutes.
- If you leave out `--ram-size` and `--cpu-count`, Anka will choose for you based on your total CPU and RAM. It will try to halve the total CPU and RAM values, but has a min of 2CPU/4GB and a max of 8CPU/8GB.
- By default `anka create` creates a VM Template with the username `anka` and password `admin`. Environment variable are available to change these: `ANKA_DEFAULT_USER` and `ANKA_DEFAULT_PASSWD` (be sure to use `sudo -E` when issuing the create command).
- SIP is DISABLED by default inside of Anka 2 VMs. This can be re-enabled with `anka modify {vmNameOrUUID} set custom-variable sys.csr-active-config 0`.

{{< hint warning >}}
Suspending VMs can sometimes produce a VM which is frozen on start. Usually this is because the hardware & cpu type you created the VM and suspended it on is different from the one you're trying to start it on. Be sure to suspend your VMs on the same hardware that will be running VMs. You can use `ANKA_CREATE_SUSPEND=0 anka create . . .` to produce a stopped VM.
{{< /hint >}}

{{< hint info >}}

- **Anka Develop license (default):** While you can create as many VMs as you wish, the free Anka Develop license only allows you to run one VM at a time and will only function on laptops (Macbook, Macbook Pro, and Macbook Air). It only supports a stopped VM state.

- **Anka Build license:** When determining how many vcpus and ram your VM needs, you can divide the number of VMs you plan on running simultaneously within a host by the total **virtual cores (vcpus)** it has. So, if I have 12vCPUs on my 6core Mac Mini, and I want to allow 2 running VMs at once and not cripple the host machine, I will set the VM Template/Tag to have 6vcpus (12 / 2). However, with RAM, you'll need to allow ~2GB of memory for the Anka Software and host ((totalRAM / 2)-1). Build licenses support suspended and stopped VM states.

    <!-- 2. Pushing and pulling your VM from the registry can be optimized by setting `anka config chunk_size 2147483648` on the node you create your templates on. It must be set before you create the VM. [More about the chunking feature]({{< relref "intel/Whats New/_index.md#registry-pushing-and-pulling-of-vm-templatestags-are-now-chunked-for-better-performance" >}}) -->
{{< /hint >}}

<!-- {{< hint info >}}
If you're using `--postinstall` and your script is failing, you can see STDOUT/ERR inside of `/var/log/install.log`. This log file is found inside of the VM.
{{< /hint >}} -->

<!-- {{< hint info >}}
On Catalina and higher, the `--postinstall` requires a package and will no longer accept scripts like Mojave did. [We have a script that can convert your scripts to pkg files here.](https://github.com/veertuinc/scripts/blob/main/script2pkg.sh)
{{< /hint >}} -->
