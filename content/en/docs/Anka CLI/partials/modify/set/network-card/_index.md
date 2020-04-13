```shell
> sudo anka modify 10.15.4 set network-card --help
Usage: anka modify set network-card [OPTIONS] [INDEX]...

  modify network card settings

Options:
  -t, --type [shared|host|bridge|disconnected]
  -b, --bridge TEXT               host interface name to bridge with (Wi-Fi is not supported)
  -m, --mac TEXT                  specify fixed MAC address
  -n, --no-mac                    deassign fixed MAC address
  --help                          Show this message and exit.
```
