
---
date: 2019-12-12T00:00:00-00:00
title: "Using real devices attached to Anka VMs for CI"
linkTitle: "Using real devices attached to Anka VMs for CI"
weight: 10
description: >
  How to work with real devices.
---



If you run tests on real devices connected to your mac machines, then you can replicate the same setup on Anka Build macOS private CI cloud with USB features available in the enterprise tier.


1. Attach real devices to Anka Build node host mac machines through the USB interface.

2. Establish trust between the device and the host. To access iOS devices from the VM the device and VM have to be trusted.

3. The trust procedures are the same in the VM and host, but is the device is not physically detached from the USB port, connection to VM could be treated as a security attack, and the device won’t respond to VM’s requests. In this case, it’s recommended to not “trust” host on iOS devices before it's done for VM. If the host is already trusted before passing the iOS device to VM, it’s recommended to clear the pairing information by Settings->General->Reset->Reset Network Settings. For a more detailed explanation see this [blog](https://veertu.com/ios-12-usb-pairing-with-anka-vm-for-ios-ci/).

4. Claim the devices using `anka usb claim` command. If you have multiple device types like iPhone7, iPhone8, Apple watch then claim each type across your Anka Build node host mac machines under the same name. For example, claim all iPhone7 devices under claim name “iPhonedevice” using the `anka usb claim -n` flag, Apple watch under “watchdevice”. Then, use this claim name along with VM start command to get access to a VM with the device attached.

5. Use the controller REST API call `url= "/api/v1/usb"` to get a list of all real devices/USB devices connected to the Anka Build cloud.

6. Start VMs on-demand with a real device attached using command `anka start -d` or start from the controller REST API using `url= "/api/v1/vm"` and pass the USB_device variable.


## USB command reference
Please refer to [USB commands page]({{< relref "usb-commands.md" >}}).

