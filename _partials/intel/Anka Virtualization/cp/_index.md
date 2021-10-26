```shell
> sudo anka cp --help
Usage: anka cp [OPTIONS] [SRC]... DST

  Copy files in and out of the VM and host

Options:
  -R      Copy the SRC and its entire subtree to the DST (required for folders)
  -L      All symbolic links within your SRC directory are followed
  -H      Symbolic links in the command are followed
  -P      Do not follow symbolic links (enabled by default)
  -f      Overwrite if DST already exists
  -n      Skips if object already exists at DST
  -p      Preserve the SRC object attributes
  -a      Alias for -pPR
  --help  Display usage information
```
