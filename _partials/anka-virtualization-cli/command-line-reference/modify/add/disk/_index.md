```shell
> anka modify 13.1-openjdk-11.0.14.1-jenkins add disk --help
usage: hard-drive,disk [options]

   Modify hard drive settings

options:
  -c,--controller <val>    set controller: sata/virtio-blk
  -s,--size <val>          set disk size (supported suffixes: T|G|M|K)
  -f,--file <val>          assign external image or device
  --ro                     mark the image as read-only for the VM
  --rw                     mark the image as writable (default) for the VM
```
