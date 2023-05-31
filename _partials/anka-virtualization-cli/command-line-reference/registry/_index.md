```shell
> anka registry --help
usage: registry [options] <command>

   Configure and control template registries

options:
  -r,--remote <val>        Sets an alternate registry
  --insecure               Skip TLS verification
  --cert <val>             Path to a client certificate (if user authentication is configured)
  --key <val>              Path to private key if the client certificate doesn't contain one
  --cacert <val>           Use custom CA certificates location (default is /etc/ssl/certs)
  --api-key-id <val>       UAK/TAP identity
  --api-key <val>          UAK/TAP key in PEM format (path or string)

commands:
  list-repos               List registries configured
  add                      Add a registry
  set                      Set default registry
  remove                   Remove a registry from the configuration
  list                     List templates in registry (see anka-push/pull commands)
  show                     Show a template's properties
  revert                   Delete a template or tag
```
