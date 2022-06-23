
---
---
### Authentication / Authorization (standalone registry)
{{< include file="content/_partials/Anka Build Cloud/configuration-reference/registry/authentication_authorization(standaloneregistry)/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_API_KEYS_CLEANING_INTERVAL | (duration) | The interval for cleaning of expired api keys. | 4h0m0s |
| ANKA_API_KEYS_SESSION_TTL | (duration) | The API Keys session TTL (used for automatic expiration). | 5m0s |
| ANKA_BACKEND_PLUGIN_PATH | (string) | The path to a backend plugin (instead of using disk) |  |
| ANKA_CA_CERT | (string) | (Certificate Authentication) The CA/root cert used to authenticate incoming requests/certs. |  |
| ANKA_ENABLE_API_KEYS | (boolean) | Enable API Key Authentication. | false |
| ANKA_ENABLE_AUTH | (boolean) | Enable Authentication (Root Token, Certificate, SSO/OpenID  or API Keys) (Not to be confused with Authorization). | false |
| ANKA_ENABLE_AUTHORIZATION | (boolean) | Enable Authorization for the standalone registry. | false |
| ANKA_OIDC_CLIENT_ID | (string) | (OpenID/SSO) Client id |  |
| ANKA_OIDC_GROUPS_CLAIM | (string) | (OpenID/SSO) Claim key to use for groups, defaults to groups | groups |
| ANKA_OIDC_PROVIDER_URL | (string) | (OpenID/SSO) Provider url |  |
| ANKA_OIDC_USERNAME_CLAIM | (string) | (OpenID/SSO) Claim key to use for user name, defaults to name |  |
| ANKA_ROOT_TOKEN | (string) | Sets the basic auth token that will be used for accessing the API (username is 'root'). |  |
| ANKA_USE_BACKEND_PLUGIN | (boolean) | Turns on usage of backend plugin provided by backend-plugin-path | false |
