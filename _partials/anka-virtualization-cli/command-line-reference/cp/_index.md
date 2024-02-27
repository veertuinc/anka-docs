```shell
> anka cp --help
usage: cp [options]

   Copy files in/out of the Anka VM and host:
       cp [options] <vmid>:path/inside </path/on/host>
       cp [options] </path/on/host> <vmid>:[path/inside]

options:
  -R                       copy the src and its entire subtree to the dst
  -L                       all symbolic links are followed
  -H                       symbolic links in the command are followed
  -P                       do not follow a symbolic links (default)
  -f                       remove and create the dst file on open failure
  -n                       do not overwrite an existing file.
  -p                       preserve the source file attributes
  -a                       same as -pPR options
  -v                       cause cp to be verbose, showing files as they are copied
```
