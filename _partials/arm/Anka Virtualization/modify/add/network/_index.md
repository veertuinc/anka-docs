```shell
> anka modify 12.0.1-jenkins add network --help
usage: network-card,network [options]

   Modify network card settings

options:
  -t,--mode <val>          network mode: shared/host/bridge/disconnected
  -b,--bridge <val>        host interface name to bridge with in the bridge mode, or "auto"
  -m,--mac <val>           specify fixed MAC address, or "auto"
  --direct-mac             expose --mac externally
  --no-direct-mac          do not expose --mac externally (default)
  -v,--vlan <val>          assign VLAN ID, 0 to deassign
  -c,--controller <val>    set controller: anet, virtio-net
  --local                  enable (default) inter-VM and VM-host communication
  --no-local               disable inter-VM and VM-host communication
```
