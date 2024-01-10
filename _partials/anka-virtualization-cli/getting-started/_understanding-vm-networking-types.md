

| Type | Description |
| --- | --- |
| `shared` | The default network type operating as NAT + DHCP. Every VM after the start/resume gets an IP address assigned by the internal DHCP server in range `192.168.64.2 - 192.168.64.254`. Programs inside a VM can access external networks (outside the host) and the internet directly. Also, other VMs on the host are also accessible. This mode typically works with multiple interfaces on the host. |
| `host` | It is very similar to the shared one, but the VM get IP addresses from range `192.168.128.2 - 192.168.128.254` and can’t access external networks outside of the host. |
| `bridge` | The Bridged type will cause the VM to show in the network as an individual device and receive a unique IP separate from the host. {{< rawhtml >}}<br /><br /><blockquote><p>An ENV is available to set the interface name: `ANKA_BRIDGE_NAME=en0`</p></blockquote><blockquote><p>When using the bridge, port-forwarding is not necessary as the VM will receive a unique IP that will be accessible directly to all other devices on the network.</p></blockquote><blockquote><p>By default, DHCP will not see your VM's MAC address. You'll need to enable `--mac` for the network-card: `anka modify VmName --type bridge --mac {MAC HERE}`</p></blockquote>{{< /rawhtml >}} |
| `disconnected` | The VM will have a disconnected network cable. |
