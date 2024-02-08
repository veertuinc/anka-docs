---
title: "Builds stuck in queued"
linkTitle: "Builds stuck in queued"
weight: 4
---
## Scenario

You configured Anka Build Cloud alongside the Teamcity plugin. You are now trying to run a build but it doesn't start and you are not seeing any new agents connected to Teamcity.

## Common Causes

1. Cloud Profile configuration mistakes; Missing `Server URL:` or incorrect `Agent Path:` not pointing to agent directory root. The `Server URL` may look populated, but it only works if you manually type it into the input.
1. SSH port forwarding not set correctly on VM.
1. The Anka Cloud can't run VMs (see [VM is stuck at scheduling]({{< relref "anka-build-cloud/troubleshooting/controller/vm-stuck-scheduling.md">}}))

If the controller has started the VM, you can take a look at the logs inside of the VM and look for the failure logs:

```bash
Veertu:~ root# anka run $(anka list | grep mgmtManaged | awk '{print $2}') tail -500 /Users/anka/buildAgent/logs/teamcity-agent.log
[2021-02-08 05:49:34,520]   INFO -   jetbrains.buildServer.SERVER - Starting TeamCity agent
[2021-02-08 05:49:34,522]   INFO - s.buildServer.agent.AgentMain2 -
[2021-02-08 05:49:34,522]   INFO - s.buildServer.agent.AgentMain2 - ===========================================================
[2021-02-08 05:49:34,566]   INFO - s.buildServer.agent.AgentMain2 - TeamCity Build Agent 2020.2.2 (build 85899)
[2021-02-08 05:49:34,585]   INFO - s.buildServer.agent.AgentMain2 - OS: Mac OS X, version 10.16, x86_64, Current user: anka, Time zone: PST (UTC-08:00)
[2021-02-08 05:49:34,589]   INFO - s.buildServer.agent.AgentMain2 - Java: 1.8.0_242, OpenJDK 64-Bit Server VM (25.242-b08, mixed mode), OpenJDK Runtime Environment (1.8.0_242-b08), AdoptOpenJDK; JVM parameters: -ea -Xmx384m -Dteamcity_logs=../logs/ -Dlog4j.configuration=file:../conf/teamcity-agent-log4j.xml
[2021-02-08 05:49:34,589]   INFO - s.buildServer.agent.AgentMain2 - Agent home is "/Users/anka/buildAgent"
[2021-02-08 05:49:34,589]   INFO - s.buildServer.agent.AgentMain2 - Starting...
[2021-02-08 05:49:34,651]   INFO - buildServer.agent.AgentMain2$3 - Refreshing jetbrains.buildServer.agent.AgentMain2$3@7a1ebcd8: startup date [Mon Feb 08 05:49:34 PST 2021]; root of context hierarchy
[2021-02-08 05:49:36,478]   INFO -    jetbrains.buildServer.AGENT - Calculating CPU benchmark index...
[2021-02-08 05:49:36,531]   INFO -    jetbrains.buildServer.AGENT - Agent tools download temp directory created on path /Users/anka/buildAgent/temp/tools
[2021-02-08 05:49:36,861]   INFO - dAgentConfigurationInitializer - Loading build agent configuration from /Users/anka/buildAgent/conf/buildAgent.properties
[2021-02-08 05:49:37,022]   INFO - s.buildServer.agent.AgentMain2 - Working dir: /Users/anka/buildAgent/work
[2021-02-08 05:49:37,022]   INFO - s.buildServer.agent.AgentMain2 - Temp dir: /Users/anka/buildAgent/temp
[2021-02-08 05:49:37,027]   INFO - rver.plugins.PluginManagerImpl - ===========================================================
[2021-02-08 05:49:37,027]   INFO - rver.plugins.PluginManagerImpl - Plugins initialization started...
[2021-02-08 05:49:37,027]   INFO - rver.plugins.PluginManagerImpl - Scanning plugins folders
[2021-02-08 05:49:37,028]   INFO - .plugins.files.JarSearcherBase - Scanning plugin folder: /Users/anka/buildAgent/plugins
[2021-02-08 05:49:37,030]   INFO - .plugins.files.JarSearcherBase - Scanning plugin folder: /Users/anka/buildAgent/tools
[2021-02-08 05:49:37,043]   INFO - rver.plugins.PluginManagerImpl - Found 1 non bundled plugins: [amazonEC2]
[2021-02-08 05:49:37,043]   INFO - rver.plugins.PluginManagerImpl - Found 0 bundled plugins: []
[2021-02-08 05:49:37,045]   INFO - rver.plugins.PluginsCollection - Load shared classloader for 1 plugins [amazonEC2]
[2021-02-08 05:49:37,050]   INFO - rver.plugins.PluginsCollection - No plugins were loaded with standalone classloaders
[2021-02-08 05:49:37,188]   INFO - rver.plugins.PluginManagerImpl - Plugins initialization completed (1 plugins loaded): [amazonEC2]
[2021-02-08 05:49:37,189]   INFO - rver.plugins.PluginManagerImpl - ===========================================================
[2021-02-08 05:49:37,191]   INFO -    jetbrains.buildServer.AGENT - Build Agent version: 85899, plugins signature: NA
[2021-02-08 05:49:37,374]   INFO - dAgentConfigurationInitializer - Loading build agent configuration from /Users/anka/buildAgent/conf/buildAgent.properties
[2021-02-08 05:49:40,752]   INFO - buildServer.agent.AgentMain2$3 - Closing jetbrains.buildServer.agent.AgentMain2$3@7a1ebcd8: startup date [Mon Feb 08 05:49:34 PST 2021]; root of context hierarchy
[2021-02-08 05:49:47,262]   INFO -   jetbrains.buildServer.SERVER - Starting TeamCity agent
[2021-02-08 05:49:47,264]   INFO - s.buildServer.agent.AgentMain2 -
[2021-02-08 05:49:47,264]   INFO - s.buildServer.agent.AgentMain2 - ===========================================================
[2021-02-08 05:49:47,278]   INFO - s.buildServer.agent.AgentMain2 - TeamCity Build Agent 2020.2.2 (build 85899)
[2021-02-08 05:49:47,287]   INFO - s.buildServer.agent.AgentMain2 - OS: Mac OS X, version 10.16, x86_64, Current user: anka, Time zone: PST (UTC-08:00)
[2021-02-08 05:49:47,290]   INFO - s.buildServer.agent.AgentMain2 - Java: 1.8.0_242, OpenJDK 64-Bit Server VM (25.242-b08, mixed mode), OpenJDK Runtime Environment (1.8.0_242-b08), AdoptOpenJDK; JVM parameters: -ea -Xmx384m -Dteamcity_logs=../logs/ -Dlog4j.configuration=file:../conf/teamcity-agent-log4j.xml
[2021-02-08 05:49:47,290]   INFO - s.buildServer.agent.AgentMain2 - Agent home is "/Users/anka/buildAgent"
[2021-02-08 05:49:47,290]   INFO - s.buildServer.agent.AgentMain2 - Starting...
[2021-02-08 05:49:47,326]   INFO - buildServer.agent.AgentMain2$3 - Refreshing jetbrains.buildServer.agent.AgentMain2$3@30a3107a: startup date [Mon Feb 08 05:49:47 PST 2021]; root of context hierarchy
[2021-02-08 05:49:48,831]   INFO -    jetbrains.buildServer.AGENT - Calculating CPU benchmark index...
[2021-02-08 05:49:48,884]   INFO -    jetbrains.buildServer.AGENT - Agent tools download temp directory created on path /Users/anka/buildAgent/temp/tools
[2021-02-08 05:49:49,057]   INFO - dAgentConfigurationInitializer - Loading build agent configuration from /Users/anka/buildAgent/conf/buildAgent.properties
[2021-02-08 05:49:49,092]   INFO - s.buildServer.agent.AgentMain2 - Working dir: /Users/anka/buildAgent/work
[2021-02-08 05:49:49,092]   INFO - s.buildServer.agent.AgentMain2 - Temp dir: /Users/anka/buildAgent/temp
[2021-02-08 05:49:49,095]   INFO - rver.plugins.PluginManagerImpl - ===========================================================
[2021-02-08 05:49:49,095]   INFO - rver.plugins.PluginManagerImpl - Plugins initialization started...
[2021-02-08 05:49:49,095]   INFO - rver.plugins.PluginManagerImpl - Scanning plugins folders
[2021-02-08 05:49:49,096]   INFO - .plugins.files.JarSearcherBase - Scanning plugin folder: /Users/anka/buildAgent/plugins
[2021-02-08 05:49:49,099]   INFO - .plugins.files.JarSearcherBase - Scanning plugin folder: /Users/anka/buildAgent/tools
[2021-02-08 05:49:49,113]   INFO - rver.plugins.PluginManagerImpl - Found 1 non bundled plugins: [amazonEC2]
[2021-02-08 05:49:49,113]   INFO - rver.plugins.PluginManagerImpl - Found 0 bundled plugins: []
[2021-02-08 05:49:49,116]   INFO - rver.plugins.PluginsCollection - Load shared classloader for 1 plugins [amazonEC2]
[2021-02-08 05:49:49,120]   INFO - rver.plugins.PluginsCollection - No plugins were loaded with standalone classloaders
[2021-02-08 05:49:49,244]   INFO - rver.plugins.PluginManagerImpl - Plugins initialization completed (1 plugins loaded): [amazonEC2]
[2021-02-08 05:49:49,245]   INFO - rver.plugins.PluginManagerImpl - ===========================================================
[2021-02-08 05:49:49,247]   INFO -    jetbrains.buildServer.AGENT - Build Agent version: 85899, plugins signature: NA
[2021-02-08 05:49:49,277]   INFO - dAgentConfigurationInitializer - Loading build agent configuration from /Users/anka/buildAgent/conf/buildAgent.properties
[2021-02-08 05:49:54,635]   INFO -    jetbrains.buildServer.AGENT - CPU benchmark index is set to 554
[2021-02-08 05:49:56,376]   INFO - n.AmazonInstanceMetadataReader - Amazon is not available. Amazon EC2 integration is not active: Failed to connect to http://169.254.169.254/2016-04-19. The host did not accept the connection within timeout of 7000 ms
[2021-02-08 05:49:56,416]   INFO - .agent.impl.OsArchBitsDetector - Detecting via "getconf LONG_BIT", exit code: 0, output: 64
[2021-02-08 05:49:56,416]   INFO - .agent.impl.OsArchBitsDetector - "teamcity.agent.os.arch.bits" detected as "64"
[2021-02-08 05:49:56,421]   INFO -    jetbrains.buildServer.AGENT - Start build agent
[2021-02-08 05:49:56,435]   INFO -    jetbrains.buildServer.AGENT - Agent Web server started on port 9090
[2021-02-08 05:49:56,439]   INFO - agent.impl.AgentPortFileWriter - Writing agent runtime file to /Users/anka/buildAgent/logs/buildAgent.xmlRpcPort
[2021-02-08 05:49:56,440]   INFO - agent.impl.AgentPortFileWriter - Launcher version is 85899
[2021-02-08 05:49:56,441]   INFO - agent.impl.AgentPortFileWriter - Writing agent runtime file to /Users/anka/buildAgent/logs/buildAgent.xmlRpcPort :DONE!
[2021-02-08 05:49:56,441]   INFO -    jetbrains.buildServer.AGENT - Build agent started
[2021-02-08 05:49:56,443]   INFO - buildServer.AGENT.registration - Registering on server via URL "http://localhost:8111": AgentDetails{Name='mgmtManaged-11.2-openjdk-1.8.0_242-teamcity-Veertu.local-1612792156424772000', AgentId=null, BuildId=null, AgentOwnAddress='null', AlternativeAddresses=[192.168.64.39], Port=9090, Version='85899', PluginsVersion='NA', AvailableRunners=[], AvailableVcs=[], AuthorizationToken='', PingCode='OCuiyh8e8jRM0Ir9gvipnxXmM2UlhWDW'}
[2021-02-08 05:49:56,449]   WARN - buildServer.AGENT.registration - Error while asking server for the communication protocols via URL http://localhost:8111/app/agents/protocols. Will try later: java.net.ConnectException: Connection refused (Connection refused) (enable debug to see stacktrace)
[2021-02-08 05:49:56,449]   WARN - buildServer.AGENT.registration - Error registering on the server via URL http://localhost:8111. Will continue repeating connection attempts.
[2021-02-08 05:49:56,942]   INFO - r.artifacts.impl.HttpDiskCache - Cleaning up items with life time in cache greater than 172800 seconds
```

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

