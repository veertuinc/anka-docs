```shell
> sudo anka modify 12.3.1 add network-card --help
Usage: anka modify add network-card [OPTIONS]

Options:
  -t, --type [shared|host|bridge|disconnected]
                                  [default: shared]
  -b, --bridge TEXT               Host interface name to bridge with (Wi-Fi not supported)
  -m, --mac TEXT                  Assign to a fixed MAC address
  -c, --controller [anet|virtio-net]
                                  Specify controller
  --help                          Display usage information
```
