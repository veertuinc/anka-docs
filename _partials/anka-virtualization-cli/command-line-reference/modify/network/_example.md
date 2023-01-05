```shell
❯ anka --machine-readable show 13.1 network | jq '.body'
{
  "mode": "shared",
  "controller": "virtio-net",
  "port_forwarding_rules": [
    {
      "name": "ssh",
      "protocol": "tcp",
      "guest_port": 22
    }
  ]
}

❯ anka modify 13.1 network --mode bridge

❯ anka --machine-readable show 13.1 network | jq '.body.mode'
"bridge"
```
