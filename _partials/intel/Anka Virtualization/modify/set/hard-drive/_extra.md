---
---

{{< hint info >}}
Differences between controllers:

- `ablk` (sata)
- `virtio-blk`

These devices require no drivers in guest/VM to work. With `virtio-blk`, it uses slightly less CPU during IO (a feature of macOS virtio driver) so we are creating BigSur VMs with it by default. Unfortunately in Monterey, Apple decided to introduce APFS Volume locking on top of `virtio-blk` (this is how their Virtualization.framework work with disks) and we are unable to continue use `virtio-blk` since some cryptography, which we don't emulate, is expected by Monterey kernel. Therefore, Monterey VMs are created with `ablk`.
{{< /hint >}}

{{< hint warning >}}
It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM Template with a smaller amount of available disk and then increase in subsequent Tags.
{{< /hint >}}

{{< hint warning >}}
Post-modify, you need to tell disk utility to modify the VM's disk to consume the new free space: `anka run --no-volume {vmNameOrUUID} sudo diskutil apfs resizeContainer disk1 0` (Intel only)
{{< /hint >}}