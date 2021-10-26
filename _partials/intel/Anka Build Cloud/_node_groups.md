![node groups](/images/using-controller/anka-controller-node-group-page.png) 

This feature allows users to add Anka Virtualization Nodes to groups which can then be used to limit or isolate CI/CD tasks. You can even create fallback groups should the primary group you assign to a Node reach capacity. This is useful when you have multiple projects in your organization and wish to prevent certain projects from using all available VM slots.

> Both the Controller UI and API allow creation and control of groups

> You can assign a Node to a group in the Controller Nodes UI, API, or even when joining with `ankacluster join --groups`