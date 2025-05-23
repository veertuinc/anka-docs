---
title: "Registry VM Templates and Tags"
weight: 4
---

### VM Templates and Tags

{{< include file="_partials/anka-build-cloud/vm-templates.md" >}}

## Replacing a Template in the Registry but keep its Name and UUID

Sometimes you'll want to replace an existing VM Template in the Registry with a brand new one instead of pushing a new tag with minor changes to the original Template. This is useful to fix a UUID/Template for each Team that will use VM Templates with their own dependencies installed inside. There are some important things to understand though:

- A VM Template's Tags in the Registry are versioned starting with 0 (the original tag pushed) and increase with subsequent pushes (1, 2, 3, etc).
- If Tag 0 and 1 exist, 0 cannot be deleted without deleting 1 as well. This same thing applies to three tags like 0, 1, 2, and deleting 1 will delete 2 but keep 0.

In the situation of Tag 0 being the only tag for a Template, and then wanting to replace it, you'll want to do the following:

1. Create an empty VM using `anka create TeamA`.
2. Push this to the Registry with `anka push TeamA --tag empty`

You will now have an empty VM in the registry, not consuming any space, but with a specific UUID (`8cdd7656-9d84-4c7b-bcd2-8f541ad9be11` for example). You can now push a second VM (with a totally different UUID) on top of the one in the Registry and have it adopt `8cdd7656-9d84-4c7b-bcd2-8f541ad9be11` as its UUID:

```bash
anka push --remote-vm TeamA {NEW_VM_NAME} --tag V1
```

Now if you want to replace Tag V1 with a new VM, but have it be under the same name, UUID, and even Tag name, you can revert the previous version with `anka registry revert TeamA` and push.

{{< hint warning >}}
Avoid reverting Tag 0 as it will delete the entire Template from the Registry.
{{< /hint >}}

{{< hint warning >}}
The `--remote-vm` CLI option can be dangerous as pushing a completely independent VM on top of another can orphan previous tags (if it has data) and cause your Registry to run out of disk space. Be sure to revert before pushing.
{{< /hint >}}