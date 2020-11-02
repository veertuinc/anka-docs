---
date: 2019-12-12T00:00:00-00:00
title: "Working with USB Devices"
linkTitle: "Working with USB Devices"
weight: 4
description: >
  Using the Anka CLI and API to setup and manage USB devices.
---

## Using the Anka CLI to claim USB devices on a host machine

> **When connecting to the host, DO NOT trust the device (only trust when attaching to the VM).**
> If you've already trusted the device on your host in the past, you'll need to **Reset Network Settings** on the device: `Settings -> General -> Reset -> Reset Network Settings`.

1. List USB devices and find your device and note the _name_, _vendor_id_, and _location_.
{{< highlight shell "hl_lines=11" >}}
sh-3.2# anka usb list
+------------------------------------+------------------------------------------+------------+--------------+-----------+
| name                               | vendor_id                                |   location |   is_claimed |   is_busy |
+====================================+==========================================+============+==============+===========+
| Device in Port 8                   | 05ac:8104                                | 2155872256 |            0 |         0 |
+------------------------------------+------------------------------------------+------------+--------------+-----------+
| Apple T2 Controller                | 05ac:8233                                | 2148532224 |            0 |         0 |
+------------------------------------+------------------------------------------+------------+--------------+-----------+
. . .
+------------------------------------+------------------------------------------+------------+--------------+-----------+
| iPhone                             | 275a5e5f1313b22305c9beaffc4d58d985ebxxxx |  336675856 |            0 |         0 |
+------------------------------------+------------------------------------------+------------+--------------+-----------+
{{</ highlight >}}

2. You can now claim the device using the _name_, _vendor_id_, or _location_ and optionally add it to a claim name group:
    ```shell
    anka usb claim -n iphones 275a5e5f1313b22305c9beaffc4d58d985ebxxxx
    ```

> If you have more than one iPhone, using a claim name (`--claim-name`) to group them is usually good practice. This allows you to specify `iphones` when starting a VM. **Be aware:** If you've assigned multiple devices to the claim name, the VM will nondeterministically choose a non-busy device.
    
3. Confirm that it's claimed (`is_claimed = 1`):
    ```shell
    sh-3.2# anka usb list | grep -E "name|iPhone"
    | name                               | vendor_id                                |   location |   is_claimed |   is_busy |
    | iPhone (iphones)                   | 275a5e5f1313b22305c9beaffc4d58d985ebxxxx |  336675856 |            1 |         0 |
    ```

4. Now attach the device to a VM using the _name_, _vendor_id_, _location_, or _claim_name_ (we'll use the claim name):
    ```shell
    sh-3.2# anka start --usb iphones 10.15.4
    Downloading files  [####################################]  100%
    vm pulled successfully with uuid: e56b4aaf-0797-42dd-9ebe-41908bf10a4d
    +-----------------------+---------------------------------------------+
    | uuid                  | e56b4aaf-0797-42dd-9ebe-41908bf10a4d        |

    . . .

    usb

    +--------+------------------------------------------+------------+--------------+
    | name   | vendor_id                                |   location | claim_name   |
    +========+==========================================+============+==============+
    | iPhone | 275a5e5f1313b22305c9beaffc4d58d985ebxxxx |  336675856 | iphones      |
    +--------+------------------------------------------+------------+--------------+
    ```

> Alternatively, the [`anka attach`]({{< relref "docs/Anka Virtualization/command-reference.md#attach" >}}) command is available if the VM is already running.

5. Confirm that the device is available within the VM:
{{< highlight shell "hl_lines=16" >}}
sh-3.2# anka run 10.15.4 system_profiler SPUSBDataType
USB:

    USB 3.0 Bus:

      Host Controller Driver: AppleUSBXHCIPPT
      PCI Device ID: 0x1e31
      PCI Revision ID: 0x0000
      PCI Vendor ID: 0x8086

        iPhone:

          Product ID: 0x12a8
          Vendor ID: 0x05ac (Apple Inc.)
          Version: 8.02
          Serial Number: 275a5e5f1313b22305c9beaffc4d58d985ebxxxx
          Speed: Up to 480 Mb/s
          Manufacturer: Apple Inc.
          Location ID: 0x07a00000 / 3
          Current Available (mA): 500
          Current Required (mA): 500
          Extra Operating Current (mA): 0
{{</ highlight >}}


## Using the Controller API (Requires Enterprise or higher license)

1. Perform steps outlined above to **claim** USB devices on the host machine.

2. You can now request the USB device list from the [Controller REST API.]({{< relref "docs/Anka Build Cloud/working-with-controller-and-api.md#usb">}}) The `body` object will contain an unordered list of **vendor_id** OR claim group names.

    ```shell
    anka usb claim 275a5e5f1313b22305c9beaffc4d58d985ebxxxx
    device claimed successfully

    curl https://127.0.0.1:8080/api/v1/usb                                                
    {"status":"OK","message":"","body":{"275a5e5f1313b22305c9beaffc4d58d985ebxxxx":{"busy":0,"available":1}}}

    anka usb release 275a5e5f1313b22305c9beaffc4d58d985ebxxxx
    device released successfully

    anka usb claim --claim-name iphones 275a5e5f1313b22305c9beaffc4d58d985ebxxxx
    device claimed successfully

    curl https://127.0.0.1:8080/api/v1/usb
    {"status":"OK","message":"","body":{"iphones":{"busy":0,"available":1}}}
    ```

3. Finally, start a VM using `/api/v1/vm` and set the usb_device to the device _name_ or _claim name_ (support for vendor_id is pending):

    ```shell
    curl -X POST -H "Content-Type: application/json" https://127.0.0.1:8080/api/v1/vm \
    -d '{"vmid": "e56b4aaf-0797-42dd-9ebe-41908bf10a4d", "tag": "clean", "usb_device": "iphones"}' | jq                                
    {
      "status": "OK",
      "message": "",
      "body": [
        "43189649-99e3-458e-47ae-ce40fab2b676"
      ]
    }
    ```
    - If [certificate based authentication]({{< relref "docs/Anka Build Cloud/Advanced Security Features/certificate-authentication.md" >}}) is enabled, you need to include the _--cert_, _--key_, and _--cacert_:

        ```shell
        curl https://127.0.0.1:8080/api/v1/usb --cert ~/node-crt.pem --key ~/node-key.pem --cacert ~/anka-ca-crt.pem
        {"status":"OK","message":"","body":{"275a5e5f1313b22305c9beaffc4d58d985ebxxxx":{"busy":0,"available":1}}}
        ```

4. Finally, confirm the Instance was created and started on your Controller Instances page.

> You might see a delay in receiving a Trust prompt on your device after starting the VM.