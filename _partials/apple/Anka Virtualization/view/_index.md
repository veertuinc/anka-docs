```shell
> anka view --help
usage: view [options] vmid

   Open VM display

arguments:
  vmid                     VM to view

options:
  -d,--display <val>       Specify the display(s) to view
  -s,--screenshot          Take PNG screenshot
  --pbpaste                Get the VM's pasteboard
  --pbcopy                 Send stdin to the VM's pasteboard
  --click <val>            Send HID events
  -o,--output <val>        Specify output file for the view operations
```
