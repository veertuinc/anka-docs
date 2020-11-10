```bash
❯ anka cp --help
Usage: anka cp [OPTIONS] [SRC]... DST

  Copy files in and out of the VM and host

Options:
  -R      copy the src and its entire subtree to the dst
  -L      all symbolic links are followed
  -H      symbolic links in the command are followed
  -P      do not follow a symbolic links (default)
  -f      remove and create the dst file on open failure
  -n      do not overwrite an existing file.
  -p      preserve the source file attributes
  -a      same as -pPR options
  --help  Display usage information
```