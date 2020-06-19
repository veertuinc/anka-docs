```shell
> sudo anka modify 10.15.4 set display --help
Usage: anka modify set display [OPTIONS]

  configure displays

Options:
  -c, --count INTEGER    configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         prompt for VNC password
  -v, --vnc TEXT         configure VNC
  --no-vnc               disable VNC access to the VM
  -d, --dpi INTEGER      set DPI (default is 72)
  -r, --resolution TEXT  set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Show this message and exit.
```
