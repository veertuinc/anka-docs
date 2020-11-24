```shell
> sudo anka registry pull --help
Usage: anka registry pull [OPTIONS] VM_ID

  Pull the specified VM template (and latest tag) from the registry

Options:
  -t, --tag TEXT
  -v, --version INTEGER
  -l, --local            Avoid pulling the tag from the registry if it already exists on the current machine
  -s, --shrink           Remove all other local tags to optimize disk usage
  --help                 Display usage information
```
