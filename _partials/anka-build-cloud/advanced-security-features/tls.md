---
---

#### Create a self-signed cert for the services (optional)

{{< hint warning >}}
Certificates should be in `PEM (PKCS #8)` format.
{{< /hint >}}

{{< hint warning >}}
Ensure your certs are decrypted! They cannot have passwords.
{{< /hint >}}

{{< hint info >}}
For this guide, we're running the Controller & Registry locally, so we use 127.0.0.1. If you're running the registry on a different IP, and especially in your certs only allow specific IPs, you'll need to set ANKA_REGISTRY_LISTEN_ADDRESS to the `IP:PORT` vs the default `:PORT`.
{{< /hint >}}

If you do not have TLS certificates for your Controller & Registry from a signed source, you can create them using your own CA:

```shell
export CONTROLLER_ADDRESS="127.0.0.1"
export REGISTRY_ADDRESS=$CONTROLLER_ADDRESS
openssl genrsa -out anka-controller-key.pem 4096
openssl req -new -nodes -sha256 -key anka-controller-key.pem -out anka-controller-csr.pem -subj "/O=MyGroup/OU=MyOrgUnit/CN=MyUser" \
  -reqexts SAN -extensions SAN \
  -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nextendedKeyUsage = serverAuth\nsubjectAltName=IP:$CONTROLLER_ADDRESS"))
openssl x509 -req -days 365 -sha256 -in anka-controller-csr.pem -CA anka-ca-crt.pem -CAkey anka-ca-key.pem -CAcreateserial \
  -out anka-controller-crt.pem -extfile <(echo subjectAltName = IP:$CONTROLLER_ADDRESS)
```

{{< hint info >}}
You can use the same certificate for both the Controller and Registry.
{{< /hint >}}

{{< hint info >}}
Beginning in Controller version 1.12.0, [you can control the allowed TLS Cipher Suites and minimum/maximum TLS versions]({{< relref "anka-build-cloud/configuration-reference.md#https--tls" >}}).
{{< /hint >}}

<!-- Next, ensure that the certificate has **Signature Algorithm: sha256WithRSAEncryption** using `openssl x509 -text -noout -in ~/anka-controller-crt.pem | grep Signature` (https://support.apple.com/en-us/HT210176) -->

### Configure the services to use the TLS cert

#### MacOS combined Controller & Registry package

Edit `/usr/local/bin/anka-controllerd`:

1. Change the listen address to `443`: `export ANKA_LISTEN_ADDR=":443"`

{{< hint info >}}
SSL will actually work on any port you want.
{{< /hint >}}

2. Add the following ENVs to enable HTTPS:

    ```shell
    # SSL + Cert Auth
    export ANKA_USE_HTTPS="true"
    export ANKA_SERVER_CERT="/Users/MyUser/anka-controller-crt.pem"
    export ANKA_SERVER_KEY="/Users/MyUser/anka-controller-key.pem"
    ```

3. Ensure `https` is in the registry URL:

    ```shell
    export ANKA_ANKA_REGISTRY="https://anka.registry:8089"
    ```

{{< hint warning >}}
The Controller & Registry runs as root. This is why you need to specify the absolute path to the location where you generated or are storing your certs.
{{< /hint >}}

#### Linux/Docker Controller & Registry package

Within the `docker-compose.yml`:

1. Change the **anka-controller** ports from `80:80` to `443:80`. You can keep the **anka-registry** ports the same (default: 8089).
2. Under the **anka-controller**, modify or set **ANKA_ANKA_REGISTRY** to use `https://`.
3. Ensure there is a `volumes` item that points the local cert location inside of the container at `/mnt/cert`.

Now let’s configure the Controller & Registry containers/services to use those certificates:

```bash
version: '2'
services:
  anka-controller:
    container_name: anka-controller
    build:
      context: controller
    ports:
      - "443:80"
    depends_on:
      - etcd
      - anka-registry
    restart: always
    volumes:
      - "/opt/secure/certs:/mnt/certs"
    environment:
      ANKA_ANKA_REGISTRY: "https://anka.registry:8089"
      ANKA_USE_HTTPS: "true"
      ANKA_SKIP_TLS_VERIFICATION: "true" # Only needed if registry cert is self-signed
      ANKA_SERVER_CERT: "/mnt/certs/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/certs/anka-controller-key.pem"
  anka-registry:
    container_name: anka-registry
    build:
      context: registry
    ports:
      - "8089:8089"
    restart: always
    volumes:
      - "/opt/anka-storage:/mnt/vol"
      - "/opt/secure/certs:/mnt/certs"
    environment:
      ANKA_USE_HTTPS: "true"
      ANKA_SERVER_CERT: "/mnt/certs/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/certs/anka-controller-key.pem"
```

{{< hint warning >}}
For the standalone package (separate docker containers for the controller and registry): If the SERVER_CERT and KEY is self-signed, you will need to set `ANKA_SKIP_TLS_VERIFICATION` to `true` in the controller config so it can connect to the registry.
{{< /hint >}}

### Test the Configuration

Start or restart your Controller and/or Registry and test the new TLS configuration using `https://`. You can also try using `curl -v https://$CONTROLLER_OR_REGISTRY_URL/api/v1/status`.

If that doesn’t work, try to repeat the above steps and validate that the file names and paths are correct. If you are still having trouble, debug the system as explained in the Debugging Controller section.


## Answers to Frequently Asked Questions

- Load balancers must also have both the root CA and intermediate certificates in order to function properly.
