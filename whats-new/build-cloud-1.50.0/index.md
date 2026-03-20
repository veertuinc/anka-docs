---
date: 2026-03-20T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.50.0"
---

### Force CPU/RAM Overrides in Resource Mode

Prior to 1.50.0, the Anka Cluster Agent would not consider vcpu and ram overrides when Resource Mode is enabled and it goes to determine if the template/vm can run on a host.

Starting in 1.50.0, you can now specify `ankacluster join --force-overrides-in-resource-mode . . .` to enable this behavior.

### Change VM Name Prefix

Prior to 1.50.0, the Anka Cluster Agent would prefix VM names with `mgmtManaged-`.

Starting in 1.50.0, you can now specify `ankacluster join --vm-prefix . . .` to change this behavior. The prefix can not contain spaces. Specifying empty prefix will result in using default `mgmtManaged-` prefix.

