> It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM Template with a smaller amount of available disk and then increase in subsequent Tags.

> Post-modify, you need to tell disk utility to modify the VM's disk to consume the new free space: `anka run --no-volume {vmNameOrUUID} sudo diskutil apfs resizeContainer disk1 0`