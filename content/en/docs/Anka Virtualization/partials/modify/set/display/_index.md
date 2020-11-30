```shell
> sudo anka modify 11.0.1 set display --help
Usage: anka modify set display [OPTIONS]

  Configure displays

Options:
  -c, --count INTEGER    Configure number of displays (2 max)
  --headless             same as --count 0
  -p, --password         Prompt for VNC password
  -v, --vnc TEXT         Configure VNC
  --no-vnc               Disable VNC access to the VM
  -d, --dpi INTEGER      Set DPI (default is 72)
  -r, --resolution TEXT  Set resolution, e.g. 1024x768 or 'default' to reset to default
  --help                 Display usage information
```
