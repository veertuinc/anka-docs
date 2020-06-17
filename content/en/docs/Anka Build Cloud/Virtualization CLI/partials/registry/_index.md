```shell
> sudo anka registry --help
Usage: anka registry [OPTIONS] COMMAND [ARGS]...

  VMs registry

Options:
  -r, --remote TEXT           Use repository name instead of 'default'
  -a, --registry-path TEXT    Use repository URL instead of 'default'
  -c, -i, --cert, --pem PATH  Client certificate if required
  -k, --key PATH              Private key, if the client certificate doesn't contain one
  --cacert, --root-cert PATH  Alternate ca_bundle
  --insecure                  Skip TLS verification
  --help                      Show this message and exit.

Commands:
  add                  Add repository
  check-download-size  Returns size of the data to be downloaded
  delete               Forget repository
  describe             Shows VM description
  list                 List VMs in repository
  list-repos           List registry repositories
  pull                 Pull certain VM
  push                 Push VM version (tag) to repository
  set                  Set default registry
```
