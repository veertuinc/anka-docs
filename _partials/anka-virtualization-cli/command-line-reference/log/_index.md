```shell
> anka log --help
usage: log [options] <command>

   Show and manage the logs

options:
  -s,--since <val>         Handle logs since the date provided (last hour by default)
  -t,--till <val>          Handle logs before the date provided
  -a,--all                 Handle all logs
  -l,--level <val>         Show logs of the level provided and above (warning by default)

commands:
  clean                    Clean old log files (1 year if the --till parameter isn't specified)
```
