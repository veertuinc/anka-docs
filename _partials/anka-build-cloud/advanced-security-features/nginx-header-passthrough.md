---
---

{{< hint warning >}}
Available in Controller >= 1.33.0
{{< /hint >}}

Customers who rely on NGINX Ingress controllers which terminate the HTTPS connection inside of the cluster will be able to now pass through TLS headers used for Controller & Registry communication with [Certificate Auth]({{< relref "anka-build-cloud/Advanced Security Features/certificate-authentication.md" >}}).

- This feature requires `ANKA_ENABLE_AUTH: "true"` and `ANKA_ENABLE_INGRESS_NGINX: "true"` env vars or corresponding flags to be specified. When these flags are enabled controller/registry will check `Ssl-Client-Cert` and `Ssl-Client-Verify` headers (enabled by `nginx.ingress.kubernetes.io/auth-tls-pass-certificate-to-upstream: true`). For successful authentication, `Ssl-Client-Verify` has to be exactly `SUCCESS` and `Ssl-Client-Cert` has to contain an url encoded pem certificate.
- The ingress controller should be configured to perform SSL client authentication on its own -- See [here](https://kubernetes.github.io/ingress-nginx/examples/auth/client-certs/) and [here](https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md#client-certificate-authentication). Note that its required to have `nginx.ingress.kubernetes.io/auth-tls-pass-certificate-to-upstream` set to `true`. This will enable passing of client certificate to the controller/registry.
- This whole setup implies that ingress controller performs SSL termination of all traffic inside the cluster is regular unencrypted HTTP, controller/registry will not validate received certificate, it will trust it by default, because all traffic authentication/validation is performed by nginx.

Here is an example ingress config:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: anka-controller-registry-ingress
  annotations:
     nginx.ingress.kubernetes.io/auth-tls-verify-client: "on"
     nginx.ingress.kubernetes.io/auth-tls-pass-certificate-to-upstream: "true"
     nginx.ingress.kubernetes.io/auth-tls-secret: "default/ca-secret"
     nginx.ingress.kubernetes.io/auth-tls-verify-depth: "1"
     nginx.ingress.kubernetes.io/auth-tls-error-page: "http://ingress-certificate-validation-failed.local"
```
