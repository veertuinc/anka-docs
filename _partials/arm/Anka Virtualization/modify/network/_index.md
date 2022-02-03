```shell
> anka modify 12.2.0-arm network --help
usage: network [options]

   Modify network card settings

options:
  -t,--mode <val>          network mode: shared/host/bridge/disconnected
  -b,--bridge <val>        host interface name to bridge with in the bridge mode, or "auto"
  -m,--mac <val>           specify fixed MAC address, or "auto"
  -v,--vlan <val>          assign VLAN ID, 0 to deassign
  -c,--controller <val>    set controller: anet, virtio-net
  --local                  enable (default) inter-VM and VM-host communication
  --no-local               disable inter-VM and VM-host communication
```
