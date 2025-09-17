```shell
> anka modify null add network --help
usage: network-card,network [options]

   Modify network card settings

options:
  -t,--mode <val>          network mode: shared/host/bridge/disconnected
  -b,--bridge <val>        host interface name to bridge with in the bridge mode, or "auto"
  -m,--mac <val>           specify fixed MAC address, or "auto"
  -n,--bond <val>          use bonding of [2-4] interfaces
  -v,--vlan <val>          assign VLAN ID, 0 to deassign
  -c,--controller <val>    set controller: anet, virtio-net
  --local                  allow vm-host connections (default)
  --no-local               prevent any vm-host connections
  -f,--filter <val>        filtering rules file, embed it into VM configuration with '-f- < rules.conf'), or use '-f off' to disable
```
