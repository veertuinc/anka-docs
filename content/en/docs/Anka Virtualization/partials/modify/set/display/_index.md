```shell
> sudo anka modify test set display --help
Usage: anka modify set display [OPTIONS]

  Configure displays

Options:
  -n, --count INTEGER         Configure number of displays (2 max)
  --headless                  same as --count 0
  -p, --password              Prompt for VNC password
  -v, --vnc TEXT              Configure VNC
  --no-vnc                    Disable VNC access to the VM
  -d, --dpi INTEGER           Set DPI (default is 72)
  -r, --resolution TEXT       Set resolution, e.g. 1024x768 or 'default' to reset to default
  -f, --fps INTEGER           Set refresh rate (30fps by default)
  -c, --controller [fbuf|pg]  Set video controller
  --host-gpu-location TEXT    Specify location of the host GPU to use, or 'any'
  --host-gpu-name TEXT        Specify name of the host GPU to use, or 'any'
  --features INTEGER          Specify extended features flags as a bit mask
  --help                      Display usage information
```
