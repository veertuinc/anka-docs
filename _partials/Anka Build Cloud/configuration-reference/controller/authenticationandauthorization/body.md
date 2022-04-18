
---
---
### Authentication and Authorization
{{< include file="content/_partials/Anka Build Cloud/configuration-reference/controller/authenticationandauthorization/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_API_KEY_FILE | (string) | API Key client file used for authentiation. Takes precedence over api-key-string |  |
| ANKA_API_KEY_ID | (string) | API Key client id used for authentiation |  |
| ANKA_API_KEY_STRING | (string) | API Key client string used for authentiation |  |
| ANKA_API_KEYS_CLEANING_INTERVAL | (duration) | Interval for cleaning of expired api keys | 4h0m0s |
| ANKA_API_KEYS_SESSION_TTL | (duration) | API Keys session time before expiration | 5m0s |
| ANKA_CA_CERT | (string) | (Certificate Authentication) The CA/root cert used to authenticate incoming requests/certs |  |
| ANKA_CLIENT_CERT | (string) | (Certificate Authentication) The Controller will use this when making http requests, mainly to the Registry |  |
| ANKA_CLIENT_CERT_KEY | (string) | (Certificate Authentication) The Controller will use this when making http requests, mainly to the Registry |  |
| ANKA_CLIENT_KEYPASS | (string) | (Certificate Authentication) Password for certificate and keystore (optional) |  |
| ANKA_CLIENT_KEYSTORE | (string) | (Certificate Authentication) A client keystore file in pkcs12 format; The Controller will use this when making http requests, mainly to the Registry |  |
| ANKA_ENABLE_API_KEYS | (boolean) | Enable API Key Authentication | false |
| ANKA_ENABLE_AUTH | (boolean) | Enable Authentication (Root Token, Certificate, SSO/OpenID or API Keys) | false |
| ANKA_ETCD_CA_CERT | (string) | (Etcd Certificate Authentication) The Etcd client will use this when connecting to the cluster |  |
| ANKA_ETCD_CERT | (string) | (Etcd Certificate Authentication) The Etcd client will use this when connecting to the cluster |  |
| ANKA_ETCD_CERT_KEY | (string) | (Etcd Certificate Authentication)The Etcd client will use this when connecting to the cluster |  |
| ANKA_ETCD_PASSWORD | (string) | (Etcd Certificate Authentication) Etcd Password to use for login |  |
| ANKA_ETCD_USERNAME | (string) | (Etcd Certificate Authentication) Etcd Username to use for login |  |
| ANKA_OIDC_CLIENT_ID | (string) | (OpenID/SSO) Client id |  |
| ANKA_OIDC_DISPLAY_NAME | (string) | (OpenID/SSO) Name to display on login page |  |
| ANKA_OIDC_GROUPS_CLAIM | (string) | (OpenID/SSO) Claim key to use for groups, defaults to groups | groups |
| ANKA_OIDC_PROVIDER_URL | (string) | (OpenID/SSO) Provider url |  |
| ANKA_OIDC_USERNAME_CLAIM | (string) | (OpenID/SSO) Claim key to use for user name, defaults to name |  |
| ANKA_ROOT_CERT | (string) | (Certificate Authentication) Alias of ca-cert |  |
| ANKA_ROOT_TOKEN | (string) | Sets the root token that will be used for accessing the Controller UI |  |
| ANKA_SKIP_ETCD_TLS_VERIFICATION | (boolean) | (Etcd Certificate Authentication) Don't verify Etcd TLS certificates (for self signed certificates) | false |
| ANKA_USE_ETCD_LOGIN | (boolean) | (Etcd Certificate Authentication) Enable Etcd client login with username and password | false |
| ANKA_USE_ETCD_TLS | (boolean) | (Etcd Certificate Authentication) Use TLS certificaates for authentication with etcd cluster | false |
