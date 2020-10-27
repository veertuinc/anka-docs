You can enable Event Logging to create a log of actions performed in the controller. The logs are saved under:

- Mac: `/Library/Application\ Support/Veertu/Anka/registry/files/central-logs/`
- Docker (registry container): `/mnt/vol/files/central-logs`

![event logging](/images/using-controller/event-logging.png) 

| Logged Events | Type |
| ----- | ----- |
| StartVM | apiAction |
| VMStarted | systemAction |
| VMFailedToStart | systemAction |
| TerminateVM | apiAction |
| VMTerminated | systemAction |
| DistributeTemplateToNodes | apiAction |
| TemplatePulled | systemAction |
| DeleteRegistryVm | apiAction |
| ConfigNode | apiAction |
| DeleteNode | apiAction |
| NodeUnregistered | systemAction |

You can also specify an endpoint that consumes JSON and the controller will push the events.