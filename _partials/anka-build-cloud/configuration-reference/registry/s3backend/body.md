
---
---
### S3 backend
{{< include file="content/_partials/anka-build-cloud/configuration-reference/registry/s3backend/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_API_KEYS_CLEANING_INTERVAL | (duration) | The interval for cleaning of expired api keys. | 4h0m0s |
| ANKA_API_KEYS_SESSION_TTL | (duration) | The API Keys session TTL (used for automatic expiration). | 5m0s |
| ANKA_BACKEND_S3_ABORT_INCOMPLETE_MULTIPART_UPLOAD_DAYS | (string)  | Adds a lifecycle rule to abort incomplete multipart uploads after the specified number of days. Zero means no rule will be added and an existing rule will be removed if any | 1 |
| ANKA_BACKEND_S3_BUCKET | (string) | The S3 bucket to use for the registry. | anka-registry |
| ANKA_BACKEND_S3_DOWNLOAD_BUFFER_SIZE | (int) | The buffer size in bytes to use for S3 downloads (min 4kb). | 2097152 |
| ANKA_BACKEND_S3_LOG_REQUESTS | (boolean) | Log requests to S3 service. | false |
| ANKA_BACKEND_S3_UPLOAD_CONCURRENCY | (int) | The number of concurrent uploads to S3 for a single object (min 1). Raising this value noticeably increases memory consumption. | 2 |
| ANKA_BACKEND_S3_UPLOAD_PART_SIZE | (int) | The buffer size in bytes to use for S3 uploads (min 5mb, max 500mb). Defines max template size that can be uploaded to S3: 20mb(PartSize) * 10'000(MaxParts) = 200gb. Raising this value noticeably increases memory consumption. | 20971520 |
| ANKA_ENABLE_API_KEYS | (boolean) | Enable API Key Authentication. | false |
| ANKA_ENABLE_INGRESS_NGINX | (boolean) | Enable Authentication based on headers set by Ingress Nginx (https://kubernetes.github.io/ingress-nginx/examples/auth/client-certs/ | false |
