You can enable Event Logging to create a log of actions performed in the controller. The logs are saved under:

- Mac: `/Library/Application\ Support/Veertu/Anka/registry/files/central-logs/`
- Docker (registry container): `/mnt/vol/files/central-logs`

![event logging](/images/using-controller/event-logging.png) 

You can also specify an endpoint that consumes JSON and the controller will push the events.