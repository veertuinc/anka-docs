```shell
> sudo anka modify 12.3.1 set network-card --help
Usage: anka modify set network-card [OPTIONS] [INDEX]...

  Modify network card settings

Options:
  -t, --type [shared|host|bridge|disconnected]
  -b, --bridge TEXT               host interface name to bridge with
  -m, --mac TEXT                  specify fixed MAC address
  -n, --no-mac                    deassign fixed MAC address
  --direct-mac / --no-direct-mac  expose --mac externally
  -v, --vlan INTEGER              assign VLAN ID
  -c, --controller [anet|virtio-net]
                                  set controller
  --local / --no-local            enable (default)/disable inter-VM and VM-host communication
  --help                          Display usage information
```
