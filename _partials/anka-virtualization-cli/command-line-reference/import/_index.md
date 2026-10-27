```shell
> anka import --help
usage: import [options] [file]

   Import VM from an archive

arguments:
  file                     Tarball with an exported VM/tag or filesystem layer

options:
  --fetch-only             Import VM tag without checkout
  -v,--vmid <val>          VM to import filesystem layer to
  -n,--dry-run             Just show files being imported
  --transform <val>        Use sed replace expression to transform file names
```
