---
date: 2019-12-12T00:00:00-00:00
title: "Enabling Graphics Acceleration with Apple Metal"
linkTitle: "Enabling Graphics Acceleration with Apple Metal"
weight: 3
description: >
  Using Apple's Metal/GPUs inside of your Anka VMs
---

{{< hint warning >}}
The host and guest macOS versions should be in-sync to avoid incompatibilities between the Apple metal APIs. We also recommend the latest Big Sur or higher versions.
{{< /hint >}}

Apple has included paravirtualized graphics support in Big Sur which allows us to support graphics acceleration/GPUs inside of Anka VMs.

To enable this feature, you'll need to:

1. Ensure your host is Big Sur
2. Ensure your VM is using Big Sur
3. Set the display for the VM with `anka modify {vmName} set display -c pg`