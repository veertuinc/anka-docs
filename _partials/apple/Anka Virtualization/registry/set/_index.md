```shell
> anka registry set --help
usage: set [options] name [url]

   Set default registry

arguments:
  name                     Set default registry
  url                      Registry URL if specified will be assigned to name

options:
  -f,--force               Do not perform a connectivity checks for the url
  --cert <val>             Path to a client certificate (if user authentication is configured)
  --key <val>              Path to a private key if the client certificate doesn't contain one
  --cacert <val>           Use custom CA certificates location (default is /etc/ssl/certs)
  --api-key-id <val>       TAP identity
  --api-key <val>          TAP key in PEM form
```
