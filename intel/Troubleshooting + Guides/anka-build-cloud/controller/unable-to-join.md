---
title: "Unable to join a Node using the ankacluster command"
linkTitle: "Unable to join a Node using the ankacluster command"
weight: 1
---

## Scenario

You issue the `sudo ankacluster join` command and see an error. Examples are:

```bash 
sudo -E /usr/local/bin/ankacluster join $ANKA_CONTROLLER_ADDRESS $ANKA_JOIN_ARGS
Testing connection to controller...: Ok
Testing connection to the registry...: Ok
Ok
exit status 2
Log file created at: 2021/03/04 01:42:32
Running on machine: Mac-mini
Binary: Built with gc go1.14.3 for darwin/amd64
Log line format: [IWEF]mmdd hh:mm:ss.uuuuuu threadid file:line] msg
I0304 01:42:32.983766    1280 log_cleaner.go:34] clean /var/log/veertu/anka_agent*...
I0304 01:42:32.984298    1280 log_cleaner.go:56] don't touch: /var/log/veertu/anka_agent.Mac-mini.root.log.ERROR.20210304-013843.1221
I0304 01:42:32.984303    1280 log_cleaner.go:56] don't touch: /var/log/veertu/anka_agent.Mac-mini.root.log.INFO.20210304-014232.1280
I0304 01:42:32.984306    1280 log_cleaner.go:56] don't touch: /var/log/veertu/anka_agent.Mac-mini.root.log.WARNING.20210304-013843.1221
I0304 01:42:33.001091    1280 main.go:173] starting ankaAgent
I0304 01:42:33.001106    1280 main.go:174] Anka Agent version: 1.7.1-9545c9f5
I0304 01:42:33.419606    1280 runner.go:43] Anka version is 2.3.3
I0304 01:42:33.419727    1280 runner.go:46] Agent Version is v2
E0304 01:42:33.572464    1280 base_ankactl_agent.go:94] json: cannot unmarshal object into Go struct field JsonListResponse.body of type []*cent_common.VmInfo
E0304 01:42:33.573231    1280 agent.go:298] json: cannot unmarshal object into Go struct field JsonListResponse.body of type []*cent_common.VmInfo
E0304 01:42:34.725567    1280 base_ankactl_agent.go:94] json: cannot unmarshal object into Go struct field JsonListResponse.body of type []*cent_common.VmInfo
E0304 01:42:34.725587    1280 agent.go:298] json: cannot unmarshal object into Go struct field JsonListResponse.body of type []*cent_common.VmInfo
Joining cluster failed

```

## Common Causes

1. You haven't performed `sudo anka license accept-eula`
2. The controller URL doesn't include the port.
3. The AnkaAgent is out of date. Perform `curl -O http://**{controllerUrlHere}**/pkg/AnkaAgent.pkg && sudo installer -pkg AnkaAgent.pkg -tgt /` to update it.

## Solution

Try one of the above common reasons.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)

