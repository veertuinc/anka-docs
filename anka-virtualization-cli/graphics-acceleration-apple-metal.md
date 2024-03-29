---
date: 2019-12-12T00:00:00-00:00
title: "Graphics Acceleration / Apple Metal"
weight: 8
description: >
  Using Apple's Metal inside of your Anka VMs.
---

{{< hint info >}}
MPS users: Support may be limited. Please be sure test your apps.
{{< /hint >}}

{{< hint warning >}}
Big Sur Users: The host macOS should always be either newer or the same version as VMs running PG to avoid incompatibilities between the Apple metal APIs.
{{< /hint >}}

### Intel

With Intel machines, Apple has included paravirtualized graphics support in Big Sur which allows us to support graphics acceleration/GPUs inside of Anka VMs.

To enable this feature, you'll need to:

1. Ensure your host is Big Sur (or higher)
2. Ensure your VM is using Big Sur (or higher)
3. Set the display for the VM with `anka modify {vmName} set display -c pg`

### ARM

For ARM Anka VMs, PG is enabled by default.
