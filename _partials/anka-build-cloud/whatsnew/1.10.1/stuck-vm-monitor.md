Rarely we see certain dependencies installed inside of VMs causing the entire VM to become unresponsive. Unresponsive or "stuck" VMs can run indefinitely and therefore cause CI/CD jobs to run until they hit timeouts. This is a bad experience for users, so we've added a feature which constantly checks whether or not the command-line inside of VM is responding to a simple command. If it cannot, the Anka Agent on your Node will fail the VM, causing the CI/CD job to receive a failure and then send a Termination request to the Controller. 

You can enable this feature when you join your Node to the Anka Cloud with: `sudo ankacluster join --enable-vm-monitor http://anka.controller`.

You can see the flags and options with:

```
❯ ankacluster join --help
Joins the current machine to one or many Anka Build Cloud Controllers

Usage:
  ankacluster join CONTROLLER_ADDRESS[,CONTROLLER_ADDRESS2] [flags]

Flags:
...
  --enable-vm-monitor           Enabled unresponsive VM monitoring. This will throw a failure when the VM becomes unresponsive for longer than the --vm-stuck-timeout
...
  --vm-stuck-delay duration     The time between unresponsive VM checks (default: 30s - Duration examples: 3500s, 20m, 5h)
  --vm-stuck-timeout duration   The time to wait until the VM is considered unresponsive (default: 10s - Duration examples: 3500s, 20m, 5h)
```