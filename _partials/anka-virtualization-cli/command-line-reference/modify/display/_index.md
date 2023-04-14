```shell
> anka modify 13.3.1 display --help
usage: display [options]

   Configure displays

options:
  -n,--count <val>         Configure number of displays (2 max)
  --headless               same as --count 0
  -p,--password            Deprecated, use 'anka config vnc_password' instead
  --vnc <val>              Configure VNC: on/off/address
  --no-vnc                 Disable VNC access to the VM
  -d,--dpi <val>           Set DPI (default is 72)
  -r,--resolution <val>    Set resolution, e.g. 1024x768 or 'default' to reset to default
  -f,--fps <val>           Set refresh rate (30fps by default)
  -c,--controller <val>    Set video controller: fbuf/pg
  --host-gpu-location <val>
                           Specify location of the host GPU to use, or 'any'
  --host-gpu-name <val>    Specify name of the host GPU to use, or 'any'
  --features <val>         Specify extended features flags as a bit mask
```
