```shell
❯ anka --machine-readable describe 12.0-beta | jq '.body.network_cards'
[
  {
    "mode": "shared",
    "controller": "virtio-net"
  }
]

❯ anka modify 12.0-beta network --mode bridge

❯ anka --machine-readable describe 12.0-beta | jq '.body.network_cards'
[
  {
    "mode": "bridge",
    "controller": "virtio-net"
  }
]
```