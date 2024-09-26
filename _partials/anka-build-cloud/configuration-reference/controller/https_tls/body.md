
---
---
### HTTPS / TLS
{{< include file="content/_partials/anka-build-cloud/configuration-reference/controller/https_tls/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_CIPHER_SUITES | (string)  | fmt.Sprintf(A list of cipher suites to use for HTTPS/TLS. Supported Options: %v, strings.Join(utils.GetTLSCipherSuitesNames(), , ))) |  |
| ANKA_MAX_TLS_VERSION | (string) | fmt.Sprintf(The max tls version to use with HTTPS/TLS. Supported Options: %v, strings.Join(utils.GetTLSVersions(), , ))) |  |
| ANKA_MIN_TLS_VERSION | (string) | fmt.Sprintf(The min tls version to use with HTTPS/TLS. Supported Options: %v, strings.Join(utils.GetTLSVersions(), , ))) |  |
| ANKA_SERVER_CERT | (string) | The path to a HTTPS/TLS certificate file in PEM format.) |  |
| ANKA_SERVER_KEY | (string) | The path to a HTTPS/TLS certificate private key file in PEM format.) |  |
| ANKA_SKIP_TLS_VERIFICATION | (boolean) | Disable the verification of the HTTPS/TLS certificates when making outbound requests to services (for self-signed certs).) | false |
| ANKA_USE_HTTPS | (boolean) | Enable HTTPS/TLS protocol for the controller UI and API (requires server-cert & server-key).) | false |
