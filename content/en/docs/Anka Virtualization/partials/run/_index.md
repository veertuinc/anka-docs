```shell
> sudo anka run --help
Usage: anka run [OPTIONS] VM_NAME COMMAND [ARGS]...

  Run a command inside of a VM (will start VM if suspended or stopped)

Options:
  -w, --workdir PATH              Working directory inside the VM
  -v, --volumes-from, --volume PATH
                                  Mount a host directory into VM (defaults to current directory)'--volumes-from' is
                                  deprecated form
  -n, --no-volumes-from, --no-volume
                                  Use this flag to prevent implicit mounting of current folder(see --volume option).
                                  '--no-volumes-from' is deprecated form
  -E, -e, --env                   Inherit environment variables ('-e' is the deprecated form)
  -f, --env-file PATH             Provide environment variables from file
  -N, --wait-network              Delay execution until guest network interface is fully up
  -T, --wait-time                 Delay execution until guest time syncs
  --help                          Display usage information
```
