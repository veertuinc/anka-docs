```shell
❯ anka --machine-readable describe 12.2.0-arm | jq '.body.network_cards'
[
  {
    "type": "virtio-net",
    "mode": "shared",
    "port_forwarding_rules": [
      {
        "name": "ssh",
        "protocol": "tcp",
        "guest_port": 22
      }
    ]
  }
]

❯ anka modify 12.2.0-arm network --mode bridge

❯ anka --machine-readable describe 12.2.0-arm | jq '.body.network_cards'
[
  {
    "mode": "bridge",
    "controller": "virtio-net"
  }
]
```
