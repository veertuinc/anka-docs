```shell
> anka modify 12.4 disk --help
usage: disk [options]

   Modify hard drive settings

options:
  -c,--controller <val>    set controller: ablk/virtio-blk
  -s,--size <val>          set disk size (supported suffixes: T|G|M|K)
  -f,--file <val>          assign external image or device
  --ro                     mark the image as read-only for the VM
  --rw                     mark the image as writable (default) for the VM
```
