```shell
> sudo anka modify 11.0.1 set network-card --help
Usage: anka modify set network-card [OPTIONS] [INDEX]...

  Modify network card settings

Options:
  -t, --type [shared|host|bridge|disconnected]
  -b, --bridge TEXT               host interface name to bridge with
  -m, --mac TEXT                  specify fixed MAC address
  -n, --no-mac                    deassign fixed MAC address
  -v, --vlan INTEGER              assign VLAN ID
  -c, --controller [anet|virtio-net]
                                  set controller
  --help                          Display usage information
```
