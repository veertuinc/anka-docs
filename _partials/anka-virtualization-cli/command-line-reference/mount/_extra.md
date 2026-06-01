---
---

{{< hint info >}}
Due to limitations in the way Apple allows us to mount host directories inside of the VM, we cannot guarantee that the host and guest will always have the same file contents.

## Host ⇄ Guest shared folder sync support
| Operation | Direction | Supported | Notes |
|---|---|:---:|---|
| Create new file/folder | Host → Guest | ✅ Yes | New paths appear in the guest |
| Create new file/folder | Guest → Host | ✅ Yes | New paths appear on the host |
| Read existing contents | Host → Guest | ✅ Yes | Contents present at mount time are visible |
| Modify file in place | Guest → Host | ✅ Yes | Guest edits are written through to the host |
| Modify file in place | Host → Guest | ❌ No | Guest keeps stale contents for already-accessed files (macOS virtiofs caching) |
| Delete file/folder | Guest → Host | ✅ Yes | Removal is reflected on the host |
| Delete file/folder | Host → Guest | ❌ No | Guest still sees the path after the host deletes it (cached) |
| Replace via temp + `rename()` (atomic) | Host → Guest | ✅ Yes | New inode/dentry; recommended way to update files from the host |

### Why the host → guest gaps exist

- The in-guest mount at `/Volumes/My Shared Files` is performed by **macOS's own automounter**.
- Apple's `Virtualization.framework` exposes **no API** to tune virtiofs caching (`attr_timeout`, `cache=`, DAX), so the guest caches dentries/attributes and does not re-validate when the host changes an already-seen path.
- This is a **platform limitation**, not specific to Anka — any tool built on `Virtualization.framework` behaves the same way.

### Recommendation for users
- Best practice: make changes **inside the guest** when you need them reflected on the host (fully bidirectional).
- When changing files **on the host**, prefer **creating new files** or **atomic replace** (write to a temp file, then `mv`/`rename()` over the target) instead of editing in place, so the guest picks up the change.
{{< /hint >}}

