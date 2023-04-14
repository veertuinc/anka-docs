---
---

Starting in Anka 3.3, users can use a VM/Template specific network traffic filtering which mimicks the behavior of [ipf.conf](https://man.netbsd.org/ipf.conf.5). 

{{< hint warning >}}
Filter rules are checked in descending order, with the first matching rule determining the treatment of the packet. For example, the following rules will `block any` traffic and ignore all other rules:

```text
block any
pass out from all
```
{{< /hint >}}

Examples of rules you can set on a VM:

```text
block out to 1.1.1.1 from any
block out to 1.1.1.1 port 53
block in to port 22
block out from port 68 to port 67
block in from any port 67 to any port 68
block any from port 67 to port 68
block any
block local
```

You can apply rules in several ways:

1. Globally for all VMs that run on the host by setting the path to the rules file: `anka config net_filter /Users/myUser/vm-filter-rules`. This will be ignored if the VM Template has filter rules applied already.

1. With a dynamic file from the host, set in the specific VM template, which is then applied at VM start time. This allows you to create rules specific to a VM + Host.

    ```bash
    ❯ cd ~; cat << EOF > ./rules
    pass in from 10.20.30.40
    pass out to 10.20.30.40
    block any
    EOF

    ❯ anka modify 13.3.1 network --filter rules

    ❯ anka show 13.3.1 network -f                                                                            
    pass in from 10.20.30.40
    pass out to 10.20.30.40
    block any

    ❯ cat ~/Library/Application\ Support/Veertu/Anka/vm_lib/c12ccfa5-8757-411e-9505-128190e9854e/config.yaml | grep net
    network_cards:
      controller: virtio-net
      net_filter: /Users/nathanpierce/rules
    ```

1. Embedding the rules inside of the VM's config, but not require a file on the host. This is useful to avoid having to ensure the rules file exists on each host.

    ```bash
    ❯ cd ~; cat << EOF > ./rules
    block in from any port 22
    block local
    EOF

    ❯ anka modify 13.3.1 network -f- < rules

    ❯ anka show 13.3.1 network -f           
    block in from any port 22
    block local

    ❯ cat ~/Library/Application\ Support/Veertu/Anka/vm_lib/c12ccfa5-8757-411e-9505-128190e9854e/net_filter 
    block in from any port 22
    block local%
    ```

1. You can also apply a single rule using `echo "block any" | anka modify 13.3.1 network -f-`.

{{< hint warning >}}
Applying new rules will remove all previously set.
{{< /hint >}}

{{< hint info >}}
You can disable the rules with `anka modify 13.3.1 network --filter off`.
{{< /hint >}}
