USB device control is possible using the with a non-Enterprise license. However, this doesn't allow you to control devices using the Controller API. With an Enterprise or higher license, you can attach one or more devices to your Anka VMs by making an API call:

```bash
# Claim a device using the location ID
curl -X POST "http://anka.controller/api/v1/vm" -H "Content-Type: application/json" \
  -d '{"vmid": "6b135004-0c89-43bb-b892-74796b8d266c", "count": 2, "usb_device": "336675856"}'

{
  "status": "OK",
  "message": "",
  "body": [
    "c983c3bf-a0c0-43dc-54dc-2fd9f7d62fce",
    "e74dfc0e-dc94-4ca2-575e-3219ac08ffa2"
  ]
}
```
