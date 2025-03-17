
---
---
### Authentication / Authorization (standalone registry)
{{< include file="content/_partials/anka-build-cloud/configuration-reference/registry/authentication_authorization(standaloneregistry)/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BACKEND_TYPE | (string) | The backend type to use for the registry ('disk', 's3'). | disk |
| ANKA_CA_CERT | (string) | (Certificate Authentication) The CA/root cert used to authenticate incoming requests/certs. |  |
| ANKA_CRL | (string) | (Certificate Authentication) File containing certificate revocation list (CRL) used to authenticate incoming requests/certs. |  |
| ANKA_ENABLE_AUTH | (boolean) | Enable Authentication (Root Token, Certificate, SSO/OpenID Connect or API Keys) (Not to be confused with Authorization). | false |
| ANKA_ENABLE_AUTHORIZATION | (boolean) | Enable Authorization for the standalone registry. | false |
| ANKA_ENABLE_RESOURCE_MANAGEMENT | (boolean) | Enable resource management for the standalone registry (requires enable-authorization) | false |
| ANKA_ROOT_TOKEN | (string) | Sets the basic auth token that will be used for accessing the API (username is 'root'). |  |
