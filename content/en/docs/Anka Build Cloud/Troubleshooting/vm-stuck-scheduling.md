---
title: "VM Instance is stuck at scheduling state"
linkTitle: "VM Instance is stuck at scheduling state"
weight: 1
description: >
  How to troubleshoot situations where VM Instances are stuck at scheduling state.
---


## Scenario

You are trying to start a VM Instance from the controller dashboards or rest API and the Instance is stuck in `Scheduling` state.


## Common reasons

1. None of the Nodes are active or has a valid license
2. A VM is running locally on the Node
3. The Node that is trying to run the VM can't reach the registry
4. There's not enough disk space on the Node


## None of the Nodes are active


Go to your dashboard, look at the box on the right. It should say "X More Instances Available". If this number is larger than 0, the problem is probably someplace else. Check out one of the next common reasons.


Go to your `Nodes` screen and go over the Nodes.  
If any of your Nodes has the message `Inactive (Invalid License)` under it's state, you need to go to that Node and activate the license. You can find more information about Anka License commands [here]({{< relref "docs/Anka CLI/commands.md" >}}#manage-anka-license)
If one of your Nodes has the state `Offline` it usually means that the agent running on the Node have crashed. To solve this, execute a `disjoin` and `join` commands on the Node:
```shell 
# Disjoin
sudo ankacluster disjoin                 
Disjoined the cluster

# Join
sudo ankacluster join http://localhost
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success


```
> **Note**  
> You might get the following error after performing the `disjoin`:   
> `Error:  agent not installed in domain specified`  
> If you do get this error, continue and perform `join` command.

After rejoining the Node check the dashboard for if the Node is in `Active` state. It may take about a minute for the Node to show in dashboard, so wait at least this amount. In case the Node is still in `Offline` state, contact support via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)


## A VM is running locally on the Node

*This process has to be repeated for each Node.*
Go to your node's terminal and check if any VMs are running.  
Perform 
```shell
sudo anka list
```
All VM names that are managed by the Controller should have a prefix like `mgmtManaged`. If any other VM is running you need to stop (or suspend, or delete) it. 
```shell
sudo anka stop $VM_NAME
```

After stopping the "rogue" VM the agent should start your new instance.

## The Node can't reach the registry

In order to start VMs the Node has to download them first from the Registry.  
The registry address is given to the nodes by the Controller.
You can see the Registry address configured in the upper right corner of the dashboard screen.
![Registry address](/images/anka-build/troubleshooting/reg_address.png)

*This process has to be repeated for each Node.*
Go to the Node's terminal and execute the following command (replace `http://192.168.1.105` with your registry URL):  
```shell
curl "http://192.168.1.105:8089/registry/status"
```
If everything is working you should see the following response:
```shell
{"status":"OK","body":{"status":"Running","version":"1.5.2-ce0d3271"},"message":""}
```
If there is a problem you should be seeing something similar to this:
```shell
curl: (7) Failed to connect to 192.168.1.105 port 8089: Connection refused
```

There can be many reasons for lack of communications between machines.  
The `ankacluster join` command sends a request to check the connection to the registry.
execute a `disjoin` and `join` commands on the Node:
```shell 
# Disjoin
sudo ankacluster disjoin                 
Disjoined the cluster

# Join
sudo ankacluster join http://localhost
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
Cluster join success


```

If somehow the Registry address is wrong or it has changed, write the correct external address in the configuration and restart the controller. 


## There's not enough disk space on the Node

The agent running on the Node takes care of cleaning old VMs that are not in use. It checks disk space and takes the `least recently used` cleaning approach. However, sometimes the machine's disk fills with files that are not related to VMs.  
You can check your disk space using the terminal:  
```shell
df -h
```
If your disk is running low on space, free some of it. Anka needs available disk space in order to run, even if the VM's disk writes are very little.
The Node's log files sometime take more space than it should.  
You can check the size of the log directory like this:
```shell
du -hs /var/log/veertu/
```

You can clear this directory by executing:
```shell
rm -r /var/log/veertu/*
```


## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

