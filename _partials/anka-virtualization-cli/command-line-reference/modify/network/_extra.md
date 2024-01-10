

| Type | Description |
| --- | --- |
| `shared` | The default network type operating as NAT + DHCP. Every VM after the start/resume gets an IP address assigned by the internal DHCP server in range `192.168.64.2 - 192.168.64.254`. Programs inside a VM can access external networks (outside the host) and the internet directly. Also, other VMs on the host are also accessible. |
| `host` | {{< hint warning >}}Not currently supported.{{< /hint >}} |
| `bridge` | The Bridged type will cause the VM to show in the network as an individual device and receive a unique IP separate from the host. {{< hint info >}}An ENV is available to set the interface name: `ANKA_BRIDGE_NAME=en0`{{< /hint >}}{{< hint info >}}When using the bridge, port-forwarding is not necessary as the VM will receive a unique IP that will be accessible directly to all other devices on the network.{{< /hint >}}{{< hint info >}}By default, DHCP will not see your VM's MAC address. You'll need to enable `--direct-mac` for the network-card: `anka modify VmName network --mode bridge --direct-mac`{{< /hint >}} |
| `disconnected` | The VM will have a disconnected network cable. |