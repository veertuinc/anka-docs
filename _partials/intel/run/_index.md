```shell
> sudo anka run --help
Usage: anka run [OPTIONS] VM_NAME COMMAND [ARGS]...

  Run a command inside of a VM (will start VM if suspended or stopped)

Options:
  -w, --workdir PATH   Working directory inside the VM
  -v, --volume PATH    Mount a host directory into VM
  -n, --no-volume      Use this flag to prevent implicit mounting of current folder(see --volume option and the
                       implicit_mount config paramerer).
  -E, --env            Inherit environment variables ('--env' is the deprecated form)
  -e TEXT              Provide an environment variable in overriding mode
  -f, --env-file PATH  Provide environment variables from file
  -N, --wait-network   Delay execution until guest network interface is fully up
  -T, --wait-time      Delay execution until guest time syncs
  --help               Display usage information
```
