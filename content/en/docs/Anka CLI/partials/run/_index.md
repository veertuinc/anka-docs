```shell
> sudo anka run --help
Usage: anka run [OPTIONS] VM_NAME COMMAND [ARGS]...

  Run commands inside VM environment

Options:
  -w, --workdir PATH              Working directory inside the VM
  -v, --volumes-from, --volume PATH
                                  Mount host directory (current directory by default) into VM . '--volumes-from' is
                                  deprecated form
  -n, --no-volumes-from, --no-volume
                                  Use this flag to prevent implicit mounting of current folder (see --volume option).
                                  '--no-volumes-from' is deprecated form
  -E, -e, --env                   Inherit environment variables. '-e' is deprecated form
  -f, --env-file PATH             Provide environment variables from file
  -N, --wait-network              Wait till guest network interface up
  -T, --wait-time                 Wait for guest time sync
  --help                          Show this message and exit.
```
