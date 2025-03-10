
---
---
### Authentication and Authorization
{{< include file="content/_partials/anka-build-cloud/configuration-reference/controller/authenticationandauthorization/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_API_KEYS_CLEANING_INTERVAL | (duration) | The interval for cleaning of expired api keys.) | 4h0m0s |
| ANKA_API_KEYS_SESSION_TTL | (duration) | The API Keys session TTL (used for automatic expiration).) | 5m0s |
| ANKA_CA_CERT | (string) | (Certificate Authentication) The CA/root cert used to authenticate incoming requests/certs.) |  |
| ANKA_CRL | (string) | (Certificate Authentication) File containing certificate revocation list (CRL) used to authenticate incoming requests/certs.) |  |
| ANKA_ENABLE_API_KEYS | (boolean) | Enable API Key Authentication.) | false |
| ANKA_ENABLE_AUTH | (boolean) | Enable Authentication (Root Token, Certificate, SSO/OpenID Connect or API Keys) (Not to be confused with Authorization).) | false |
| ANKA_ENABLE_CONTROLLER_AUTHORIZATION | (boolean) | Enable Authorization (Users, groups, permission control for specific certificates) in the Controller.) | false |
| ANKA_ENABLE_INGRESS_NGINX | (boolean) | Enable Authentication based on headers set by Ingress Nginx (https://kubernetes.github.io/ingress-nginx/examples/auth/client-certs/) | false |
| ANKA_ENABLE_RESOURCE_MANAGEMENT | (boolean) | Enable resource management for the controller (requires enable-controller-authorization)) | false |
| ANKA_ETCD_CA_CERT | (string) | (ETCD Certificate Authentication) The Etcd client will use this when connecting to the cluster.) |  |
| ANKA_ETCD_CERT | (string) | (ETCD Certificate Authentication) The ETCD client will use this when connecting to the cluster.) |  |
| ANKA_ETCD_CERT_KEY | (string) | (ETCD Certificate Authentication) The ETCD client will use this when connecting to the cluster.) |  |
| ANKA_ETCD_PASSWORD | (string) | (ETCD Certificate Authentication) ETCD Password to use for login.) |  |
| ANKA_ETCD_USERNAME | (string) | (ETCD Certificate Authentication) ETCD Username to use for login.) |  |
| ANKA_OIDC_CLIENT_ID | (string) | (OpenID Connect/SSO) Client id) |  |
| ANKA_OIDC_CLIENT_SECRET | (string) | (OpenID Connect/SSO) Client secret) |  |
| ANKA_OIDC_DISPLAY_NAME | (string) | (OpenID Connect/SSO) Name to display on login page) |  |
| ANKA_OIDC_GROUPS_CLAIM | (string) | (OpenID Connect/SSO) Claim key to use for groups, defaults to groups) | groups |
| ANKA_OIDC_PROVIDER_URL | (string) | (OpenID Connect/SSO) Provider URL) |  |
| ANKA_OIDC_SCOPES | (string)  | (OpenID Connect/SSO) Comma separated list of scopes, overrides default scopes used) |  |
| ANKA_OIDC_USER_INFO | (boolean) | (OpenID Connect/SSO) Get claims from user info endpoint) | false |
| ANKA_OIDC_USERNAME_CLAIM | (string) | (OpenID Connect/SSO) Claim key to use for user name, defaults to name) |  |
| ANKA_ROOT_CERT | (string) | (Certificate Authentication) Alias of ca-cert) |  |
| ANKA_ROOT_TOKEN | (string) | Sets the basic auth token that will be used for accessing the Controller UI and API (username is 'root').) |  |
| ANKA_SKIP_ETCD_TLS_VERIFICATION | (boolean) | (ETCD Certificate Authentication) Don't verify ETCD TLS certificates (for self signed certificates).) | false |
| ANKA_USE_ETCD_LOGIN | (boolean) | (ETCD Certificate Authentication) Enable ETCD client login with username and password.) | false |
| ANKA_USE_ETCD_TLS | (boolean) | (ETCD Certificate Authentication) Use TLS certificates for authentication with ETCD cluster.) | false |
