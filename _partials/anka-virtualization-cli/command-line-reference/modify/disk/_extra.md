---
---

{{< hint warning >}}
**IMPORTANT:** Changing the disk size for a clone will break image chaining/"layer sharing" between the clone and its source. You'll want to modify the source VM Template first, then clone it to a new VM.
{{< /hint >}}

{{< hint warning >}}
Post-modify, you need to tell disk utility to modify the VM's disk to consume the new free space INSIDE OF THE VM: `DISKUTIL_INFO=$(diskutil info / 2>/dev/null); APFS_CONTAINER=$(echo "${DISKUTIL_INFO}" | awk '/APFS Container:/ {print $NF; exit}'); if [ -z "${APFS_CONTAINER}" ]; then APFS_CONTAINER=$(echo "${DISKUTIL_INFO}" | awk '/APFS Physical Store:/ {print $NF; exit}'); fi; if [ -z "${APFS_CONTAINER}" ]; then APFS_CONTAINER=$(diskutil apfs list 2>/dev/null | awk '/^\+-- Container / {print $3; exit}'); fi; if [ -z "${APFS_CONTAINER}" ] || ! diskutil info "${APFS_CONTAINER}" >/dev/null 2>&1; then echo "Could not determine APFS container to resize" >&2; exit 1; fi; diskutil apfs resizeContainer "${APFS_CONTAINER}" 0`
{{< /hint >}}

{{< hint warning >}}
It's currently impossible to downsize a VM's hard-drive. We suggest creating your initial VM Template with a smaller amount of available disk and then increase in subsequent Tags.
{{< /hint >}}
