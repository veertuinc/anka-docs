

### HTTPS / TLS
{{< include file="_partials/anka-build-cloud/configuration-reference/controller/https_tls/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_CIPHER_SUITES | (string)  | A list of cipher suites to use for HTTPS/TLS. Supported Options: tls_rsa_with_aes_128_cbc_sha, tls_rsa_with_aes_256_cbc_sha, tls_rsa_with_aes_128_gcm_sha256, tls_rsa_with_aes_256_gcm_sha384, tls_aes_128_gcm_sha256, tls_aes_256_gcm_sha384, tls_chacha20_poly1305_sha256, tls_ecdhe_ecdsa_with_aes_128_cbc_sha, tls_ecdhe_ecdsa_with_aes_256_cbc_sha, tls_ecdhe_rsa_with_aes_128_cbc_sha, tls_ecdhe_rsa_with_aes_256_cbc_sha, tls_ecdhe_ecdsa_with_aes_128_gcm_sha256, tls_ecdhe_ecdsa_with_aes_256_gcm_sha384, tls_ecdhe_rsa_with_aes_128_gcm_sha256, tls_ecdhe_rsa_with_aes_256_gcm_sha384, tls_ecdhe_rsa_with_chacha20_poly1305_sha256, tls_ecdhe_ecdsa_with_chacha20_poly1305_sha256 |  |
| ANKA_MAX_TLS_VERSION | (string) | The max tls version to use with HTTPS/TLS. Supported Options: tls_1.0, tls_1.1, tls_1.2, tls_1.3 |  |
| ANKA_MIN_TLS_VERSION | (string) | The min tls version to use with HTTPS/TLS. Supported Options: tls_1.0, tls_1.1, tls_1.2, tls_1.3 |  |
| ANKA_SERVER_CERT | (string) | The path to a HTTPS/TLS certificate file in PEM format. |  |
| ANKA_SERVER_KEY | (string) | The path to a HTTPS/TLS certificate private key file in PEM format. |  |
| ANKA_SKIP_TLS_VERIFICATION | (boolean) | Disable the verification of the HTTPS/TLS certificates when making outbound requests to services (for self-signed certs). | false |
| ANKA_USE_HTTPS | (boolean) | Enable HTTPS/TLS protocol for the controller UI and API (requires server-cert & server-key). | false |
