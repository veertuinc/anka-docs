---
date: 2026-03-20T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.50.0"
---

### Force CPU/RAM overrides in Resource Mode

When a node uses `--capacity-mode resource`, the agent checks free CPU and RAM before it accepts a start request. Before 1.50.0, per-VM `cpu` and `ram` values in the start request did not count toward that check.

Pass `--force-overrides-in-resource-mode` on `ankacluster join` so overrides above zero are included in the capacity calculation:

```bash
sudo ankacluster join http://anka.controller \
  --capacity-mode resource \
  --force-overrides-in-resource-mode
```

See [Preparing and joining your nodes]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}) for the full `ankacluster join` flag list.

### Change VM name prefix

Controller-managed VMs are named `{prefix}-{name_template}`. The default prefix is `mgmtManaged` (for example, `mgmtManaged-14.2-arm64-1706114704759425000`).

Set a custom prefix when the node joins the cluster:

```bash
sudo ankacluster join http://anka.controller --vm-prefix myPrefix
```

The agent adds a dash automatically. If `name_template` is `build-123`, the VM name becomes `myPrefix-build-123`.

The `name_template` field in the [Start VM instances API]({{< relref "anka-build-cloud/working-with-controller-and-API.md#start-vm-instances" >}}) still controls the suffix. Only the prefix changes.
