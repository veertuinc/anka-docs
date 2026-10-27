---
title: "Node Capacity Modes"
linkTitle: "Node Capacity Modes"
weight: 10
description: >
  Control how many VMs an Anka Node accepts: by VM count, or by free CPU and RAM.
---

The agent on each Anka Node decides if the Node can accept a start VM task. The capacity mode sets the rule that the agent uses for this decision.

Mode | The agent accepts a start VM task when
--- | ---
`number` (default) | The number of VMs on the Node is less than the VM limit (`--max-vm-count`).
`resource` | The Node has enough free vCPUs **and** free RAM for the VM Template.

If the agent cannot accept a task, it releases the task back to the Controller queue. Another Node can then take the task. If no Node can take the task, the instance stays in the `Scheduling` state. Refer to [VM is stuck at scheduling]({{< relref "anka-build-cloud/troubleshooting/controller/vm-stuck-scheduling.md" >}}).

---

## Number Mode

Number mode is the default. The agent counts the managed VMs on the Node that are in the `running` or `failed` state. If this number is less than the VM limit, the agent accepts the task.

{{< hint info >}}
A failed VM uses a slot until you terminate the instance.
{{< /hint >}}

Number mode does not examine the CPU or RAM of the VM Template. If large VMs start on a small host, the VMs can use more CPU and RAM than the host has. This can make the host and the VMs slow.

To set the VM limit, use one of these procedures:

- Join the Node with `--max-vm-count` (default `2`).
- In the Controller UI, open the **Nodes** page and use **Change Node Configuration**.
- Send `max_vm_count` to the [Update Node API]({{< relref "anka-build-cloud/working-with-controller-and-API.md#update-node" >}}).

{{< hint warning >}}
On Apple silicon, you cannot set a VM limit of more than 2. The agent and the Controller reject the value. The `--ignore-arm-vm-limit` join flag removes this limit.
{{< /hint >}}

---

## Resource Mode

In resource mode, the agent does these steps for each start VM task:

1. It gets the CPU core count and the RAM size of the VM Template and Tag from the Registry.
1. It calculates the free vCPUs and the free RAM on the Node.
1. It accepts the task only if the free vCPUs and the free RAM are both equal to or more than the VM Template needs.

Use resource mode when your VM Templates have different sizes, or when you do not want the VMs to use more RAM than the host has.

### How the agent calculates free resources

Value | Calculation
--- | ---
Total vCPUs | The `--vcpu-override` value. If you do not set it, the number of physical CPU cores multiplied by 2. On Apple silicon, the agent counts only the performance cores.
Total RAM | The `--ram-override` value, in GB. If you do not set it, the host RAM minus 2 GB. The agent keeps the 2 GB for the host.
Free vCPUs | Total vCPUs minus the vCPUs of all managed VMs on the Node.
Free RAM | Total RAM minus the RAM of the running managed VMs on the Node.

An override value replaces the calculation. The agent does not multiply `--vcpu-override` by 2, and it does not subtract 2 GB from `--ram-override`.

{{< hint warning >}}
On Apple silicon, the limit of 2 running VMs also applies in resource mode. Resource mode prevents too many resources from being used. It does not increase the number of VMs that a Node can run. The `--ignore-arm-vm-limit` join flag removes this limit.
{{< /hint >}}

### Example

The host is an Apple silicon Mac with 8 performance cores and 32 GB of RAM. In resource mode, the total is 16 vCPUs and 30 GB of RAM. The VM Template has 8 vCPUs and 16 GB of RAM.

Start VM task | Number mode (VM limit 2) | Resource mode
--- | --- | ---
First VM | Starts. | Starts. 8 vCPUs and 14 GB of RAM stay free.
Second VM | Starts. The two VMs use 32 GB of RAM, which is all of the host RAM. | The agent rejects the task. The VM needs 16 GB, but only 14 GB is free.

In resource mode, the second task goes back to the queue. A Node with enough free resources can start it.

### CPU and RAM overrides in the start VM request

The [Start VM instances API]({{< relref "anka-build-cloud/working-with-controller-and-API.md#start-vm-instances" >}}) accepts `vcpu` and `vram` (in MB) to change the size of the VM. By default, the resource check uses the VM Template values and ignores these overrides. Because of this, a request for more RAM than the VM Template has can start on a Node that does not have that RAM free.

In version 1.50.0 and later, join the Node with `--force-overrides-in-resource-mode`. Then the resource check uses the `vcpu` and `vram` values from the request when they are more than 0.

---

## Set the Capacity Mode

### When you join the Node

```shell
sudo ankacluster join http://anka.controller \
  --capacity-mode resource
```

To set your own limits and to use the start VM request overrides in the check:

```shell
sudo ankacluster join http://anka.controller \
  --capacity-mode resource \
  --vcpu-override 12 \
  --ram-override 24 \
  --force-overrides-in-resource-mode
```

To see the mode that the agent uses, run `sudo ankacluster status` and find `capacity_mode` in the output. Refer to [Preparing and joining your nodes]({{< relref "anka-build-cloud/getting-started/preparing-and-joining-your-nodes.md" >}}) for all of the `ankacluster join` flags.

### With the Controller API

Send `capacity_mode`, `vcpu_override`, and `ram_override` to the [Update Node API]({{< relref "anka-build-cloud/working-with-controller-and-API.md#update-node" >}}). You do not have to join the Node again.

```shell
curl -X POST "http://anka.controller/api/v1/node/config" -H "Content-Type: application/json" \
  -d '{"node_id": "f8707005-4630-4c9c-8403-c9c5964097f6", "capacity_mode": "resource", "vcpu_override": 12, "ram_override": 24}'
```

{{< hint info >}}
The **Change Node Configuration** dialog in the Controller UI does not set the capacity mode. Use the API or join the Node again. You can set `--force-overrides-in-resource-mode` only when you join the Node.
{{< /hint >}}

---

## Capacity in the Controller

The [List Nodes API]({{< relref "anka-build-cloud/working-with-controller-and-API.md#list-nodes" >}}) shows `capacity_mode`, `vcpu_override`, and `ram_override` for each Node.

The meaning of the `capacity` value changes with the mode:

- In number mode, `capacity` is the VM limit.
- In resource mode, `capacity` is the number of free vCPUs on the Node. If the Node has no free RAM, `capacity` is 0.

The Controller UI shows the same value on the **Nodes** page.

---

## Troubleshooting

The agent writes the result of each capacity check to `/var/log/veertu/anka_agent.INFO`:

```shell
❯ grep -E "Resources check|capacity available" /var/log/veertu/anka_agent.INFO
... preRunCheck: capacity available: false
... Resources check to evaluate if VM can run: CPUs: required 8, available: 8; RAM: required 16 GB, available: 14 GB
```

- `capacity available: false` tells you that the Node rejected the task because of the capacity mode.
- The `Resources check` line shows the required and the available vCPUs and RAM in resource mode.

In resource mode, the Node must get the VM Template information from the Registry. If it cannot, the agent writes `failed getting vm info from registry` to the log and rejects the task.
