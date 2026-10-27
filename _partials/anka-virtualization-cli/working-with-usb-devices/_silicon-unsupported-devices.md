---
---

{{< hint warning >}}
**Devices that are not supported**

macOS does not let a VM capture a USB device that has isochronous endpoints, even when the device does not use them. This includes:

- iPhones. An iPhone has an isochronous endpoint in its audio configuration.
- Most webcams, USB audio devices, and headsets.

When you select one of these devices in the Virtual Machine Accessories menu, the VM does not get the device. The VM log shows `A USB passthrough device with isochronous endpoints is not supported.`

**Android devices disconnect and connect again**

Some Android devices disconnect from the VM and connect again approximately one time each second. We saw this with a Samsung phone. The VM gets the device, but the device does not stay connected long enough to use it. This is a macOS issue, and we reported it to Apple as FB25252156.
{{< /hint >}}
