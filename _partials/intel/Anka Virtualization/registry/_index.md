```shell
> sudo anka registry --help
Usage: anka registry [OPTIONS] COMMAND [ARGS]...

  Configure and control the Anka Cloud Registry

Options:
  -r, --remote TEXT           Set the registry name (instead of 'default')
  -a, --registry-path TEXT    Set the registry URL (instead of 'default')
  -c, -i, --cert, --pem PATH  Path to your node certificate (if certificate authentication is enabled)
  -k, --key PATH              Path to your node certificate key if the client/node certificate doesn't contain one
  --cacert, --root-cert PATH  Path to a CA Root certificate
  --insecure                  Skip TLS verification
  --help                      Display usage information

Commands:
  add                  Add a registry
  check-download-size  Return the size of the template and tag
  delete               Remove the registry from your configuration
  describe             Show a VM template description and tags
  list                 List VMs in registry
  list-repos           List registries you have configured
  pull                 Pull the specified VM template (and latest...
  push                 Push a VM template (and tag) to the Cloud...
  set                  Set default registry
```
