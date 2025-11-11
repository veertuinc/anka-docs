---
---

{{< hint warning >}}
**IMPORTANT:** Changing the disk size for a clone will break image chaining/"layer sharing" between the clone and its source. You'll want to modify the source VM Template first, then clone it to a new VM.
{{< /hint >}}

{{< hint warning >}}
Post-modify, you need to tell disk utility to modify the VM's disk to consume the new free space: `anka run {vmNameOrUUID} sudo diskutil apfs resizeContainer disk0s2 0`
{{< /hint >}}

{{< hint warning >}}
It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM Template with a smaller amount of available disk and then increase in subsequent Tags.
{{< /hint >}}
