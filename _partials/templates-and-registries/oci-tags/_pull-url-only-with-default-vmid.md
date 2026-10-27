A pull that is only a URL creates the VM for you. Point `default_vmid` at a VM you already have. A UUID is the stable choice. Add `:<tag>` to start from that tag.

```bash
❯ anka config default_vmid <vm-uuid>
❯ anka pull oci://ghcr.io/homebrew/core/cmake:4.3.4 --transform 's|cmake/4.3.4|usr/local'
```

Anka creates a VM named `cmake` from that VM and pulls the bottle into `usr/local`. You do not need to install Homebrew, add a symlink, or change `PATH`. A job that only has the registry URL can use this form.
