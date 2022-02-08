```shell
> anka registry show --help
usage: show [options] vmid <command>

   Show a template's properties

arguments:
  vmid                     Template identifier or name

options:
  -t,--tag <val>           Specify template tag, latest by default

commands:
  name                     Template name
  uuid                     Template UUID
  description              Template description
  tag                      Show template tags
  cpu                      vCPU parameters
  ram                      RAM size and parameters
  display                  Display information
  disk                     Disk information
  network                  Network information
  port                     Port forwarding rules
  label                    Assigned template labels
```
