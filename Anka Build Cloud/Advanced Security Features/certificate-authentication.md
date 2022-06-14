---
date: 2019-12-12T00:00:00-00:00
title: "Certificate Authentication"
linkTitle: "Certificate Authentication"
weight: 2
description: >
  How to set up certificate based authentication.
---

> **This guide requires an Anka Enterprise (or higher) license**


There are several different ways you can enable Certificate authentication:

1. With the combined (controller + registry) native macOS package: You'll edit the `/usr/local/bin/anka-controllerd`, enabling TLS/HTTPS (required) and then certificate authentication (with either ENVs or options/flags).
2. With the docker package: You'll edit the `docker-compose.yml`, enabling TLS/HTTPS and then certificate authentication (with ENVs).
3. With either the controller or registry standalone: You'll edit the proper config files, enabling TLS/HTTPS (required) and then certification authentication (with either ENVs or options/flags).

> If you're using the Enterprise Plus license, [you will need to setup authorization for your certificate]({{< relref "Anka Build Cloud/Advanced Security Features/certificate-authentication.md#managing-usergroup-permissions-authorization" >}})

## Requirements

1. A Root CA certificate. For more information about CAs, see https://en.wikipedia.org/wiki/Certificate_authority. Usually provided by your organization or where you obtain your certificate signing. We will refer to this as **anka-ca-crt.pem** and **anka-ca-key.pem** throughout the guide.
2. A certificate (signed with the Root CA) for the Anka Build Cloud Controller & Registry.
3. Certificates (signed with the Root CA) for your Anka Build Nodes so they can connect/authenticate with the Anka Build Cloud Controller & Registry.
4. Your keys are not password protected ("encrypted") (use `openssl rsa -in <encrypted_private.key> -out <decrypted_private.key>` to decrypt)

> **The guide will show you how to generate self-signed versions of these. If you already have certificates, you can skip the commands.**

---

## 1. Create a self-signed Root CA certificate

If you don’t have a Root CA yet, you can create it with openssl and add it to your keychain:

```shell
cd ~
openssl req -new -nodes -x509 -days 365 -keyout anka-ca-key.pem -out anka-ca-crt.pem -subj "/O=$ORGANIZATION_OR_GROUP_NAME/OU=$ORG_UNIT/CN=$COMMON_NAME_OR_USERNAME"
# Add the Root CA to the System keychain so the Root CA is trusted and you can avoid warnings when you go to access the Controller UI; if you have a better method to do this without using the system keychain, feel free to use it
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain anka-ca-crt.pem
```

## 2. Configuring TLS for Controller & Registry

> TLS must be enabled for certs to work

> Certificates should be in `PEM (PKCS #8)` format

> The **Controller TLS certificate** ("`server`" cert options) is not part of the authentication process and doesn't need to be derived from the CA you just generated. This means that you can use certificates supplied by your organization or a 3rd party for TLS/HTTPS

> Ensure your signed key is decrypted using `openssl rsa`

> For this guide, we're running the Controller & Registry locally, so we use 127.0.0.1. Update this depending on where you have things hosted

If you do not have TLS certificates for your Controller & Registry, you can create them now:

```shell
export CONTROLLER_ADDRESS="127.0.0.1"
export REGISTRY_ADDRESS=$CONTROLLER_ADDRESS
openssl genrsa -out anka-controller-key.pem 4096
openssl req -new -nodes -sha256 -key anka-controller-key.pem -out anka-controller-csr.pem -subj "/O=$ORGANIZATION/OU=$ORG_UNIT/CN=$CONTROLLER_CN" -reqexts SAN -extensions SAN -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nextendedKeyUsage = serverAuth\nsubjectAltName=IP:$CONTROLLER_ADDRESS"))
openssl x509 -req -days 365 -sha256 -in anka-controller-csr.pem -CA anka-ca-crt.pem -CAkey anka-ca-key.pem -CAcreateserial -out anka-controller-crt.pem -extfile <(echo subjectAltName = IP:$CONTROLLER_ADDRESS)
```

> You can use the same certificate for both the Controller and Registry

Ensure that the certificate has **Signature Algorithm: sha256WithRSAEncryption** using `openssl x509 -text -noout -in ~/anka-controller-crt.pem | grep Signature` (https://support.apple.com/en-us/HT210176)

> Beginning in Controller version 1.12.0, [you can control the allowed TLS Cipher Suites and minimum/maximum TLS versions]({{< relref "Anka Build Cloud/configuration-reference.md#https--tls" >}})

### Native macOS Controller & Registry package

Edit `/usr/local/bin/anka-controllerd` in the following manner:

1. Change the listen address to `443`: `export ANKA_LISTEN_ADDR=":443"`

{{< hint info >}}
SSL will actually work on any port you want.
{{< /hint >}}

2. Add the following ENVs to enable HTTPS:

    ```shell
    # SSL + Cert Auth
    export ANKA_USE_HTTPS="true"
    export ANKA_SKIP_TLS_VERIFICATION="true" # Not needed if they are signed certs
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

### Linux/Docker Controller & Registry

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
      ANKA_SKIP_TLS_VERIFICATION: "true" # Not needed if they are signed certs
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
      ANKA_SKIP_TLS_VERIFICATION: "true" # Not needed if they are signed certs
      ANKA_SERVER_CERT: "/mnt/certs/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/certs/anka-controller-key.pem"
```

### Test the Configuration

Start or restart your Controller and test the new TLS configuration using `https://`. You can also try using `curl -v https://$HOST/api/v1/status`.

If that doesn’t work, try to repeat the above steps and validate that the file names and paths are correct. If you are still having trouble, debug the system as explained in the Debugging Controller section.

## Managing User/Group Permissions (Authorization) (Enterprise Plus Only)

When creating certificates, you'll want to specify CSR values using openssl's `-subj` option. For example, if we're going to generate a certificate so our Jenkins instance can access the Controller & Registry, you'll want to use something like this:

```shell
-subj "/O=MyOrgName/OU=$ORG_UNIT/CN=Jenkins"
```

{{< include file="_partials/Anka Build Cloud/_managing-permissions.md" >}}

## 3. Creating self-signed Node Certificates

The Controller's authentication module uses the Root CA (anka-ca-crt.pem) to authenticate any incoming requests. When the Node sends the requests to the Controller, it will present its certificates. Those certificates must have been generated from the Root CA and also, if using Enterprise Plus, have the necessary permissions.

You can use the following openssl commands to create Node certificates using the Root CA:

```shell
openssl genrsa -out node-$NODE_NAME-key.pem 4096
openssl req -new -sha256 -key node-$NODE_NAME-key.pem -out node-$NODE_NAME-csr.pem -subj "/O=$ORGANIZATION/OU=$ORG_UNIT/CN=$NODE_NAME"
openssl x509 -req -days 365 -sha256 -in node-$NODE_NAME-csr.pem -CA anka-ca-crt.pem -CAkey anka-ca-key.pem -CAcreateserial -out node-$NODE_NAME-crt.pem
```

## 4. Configuring the Controller & Registry with the CA Root certificate and enable authentication

{{< hint warning >}}
The `ANKA_CA_CERT` is the authority that is used to validate the node certificates.
{{< /hint >}}

In addition to the node certificates, the controller itself makes API calls to the Registry (you've enabled registry auth, right?) to get templates, etc, and will need a client cert to communicate with it.

### Native macOS Controller & Registry package

Edit the `/usr/local/bin/anka-controllerd` and ensure the following ENVs exist:

```shell
export ANKA_ENABLE_AUTH="true"
export ANKA_CA_CERT="/Users/MyUser/anka-ca-crt.pem"
export ANKA_CLIENT_CERT="/Users/MyUser/anka-controller-crt.pem"
export ANKA_CLIENT_CERT_KEY="/Users/MyUser/anka-controller-key.pem"
```

### Linux/Docker Controller & Registry package

Within the `docker-compose.yml`, add the following ENVs:

```bash
version: '2'
services:
  anka-controller:
    container_name: anka-controller
    . . .
    environment:
      . . .
      ANKA_ENABLE_AUTH: "true"
      ANKA_CA_CERT: "/mnt/certs/anka-ca-crt.pem"
      ANKA_CLIENT_CERT: "/mnt/certs/anka-controller-crt.pem"
      ANKA_CLIENT_CERT_KEY: "/mnt/certs/anka-controller-key.pem"
  anka-registry:
    container_name: anka-registry
    . . .
    environment:
      . . .
      ANKA_ENABLE_AUTH: "true"
      ANKA_CA_CERT: "/mnt/certs/anka-ca-crt.pem"
```

{{< hint warning >}}
**Until you have an Enterprise licensed Node joined to the Controller, it won't enable authentication for the Controller.**
{{< /hint >}}

{{< hint warning >}}
If you're connecting the Anka CLI with the HTTPS Registry URL, you can use the Node certificates: `anka registry --cert /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-crt.pem --key /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-key.pem --cacert /Users/$USER_WHERE_CERTS_ARE/anka-ca-crt.pem add $REGISTRY_NAME https://$REGISTRY_ADDRESS:8089`
{{< /hint >}}

## 5. Joining your Node to the Controller with the Node certificate

{{< hint info >}}
Certificates are cached, so if you update/renew them, you need to either: 
1. disjoin and re-join them to the controller, issue `sudo pkill -9 anka_agent` on each node to restart the agent
2. or, issue a `<controller>/v1/node/update` PUT to the controller API to forcefully update all nodes.
{{< /hint >}}

{{< hint info >}}
If you're using a signed certificate for the controller dashboard, but self-signed certificates for your nodes and CI tools, you'll need to specify the `--cacert` for `ankacluster join` and `anka registry add` commands and point it to the signed CA certificate. You'll usually see `SSLError: ("bad handshake: Error([('SSL routines', 'tls_process_server_certificate', 'certificate verify failed')],)",)` if the wrong CA is being used.
{{< /hint >}}

{{< hint warning >}}
If you previously joined your Nodes to the Controller, you'll want to `sudo ankacluster disjoin` on each before proceeding (if it hangs, use `ps aux | grep anka_agent | awk '{print $2}' | xargs kill -9` and try disjoin again).
{{< /hint >}}

Copy both the Node certificates (node-$NODE_NAME-crt.pem, node-$NODE_NAME-key.pem) and the anka-ca-crt.pem to the Node.

Then, use the `ankacluster` command to connect it to the Controller in the following manner:

```shell
sudo ankacluster join https://$CONTROLLER_ADDRESS --skip-tls-verification --cert /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-crt.pem --cert-key /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-key.pem --cacert /Users/$USER_WHERE_CERTS_ARE/anka-ca-crt.pem
You should see output similar to the following:
Testing connection to Controller...: OK
Testing connection to registry….: OK
Ok
Cluster join success
```

### Testing Controller API Authentication

Restart your Controller & Registry and then test the status endpoint with curl:

```shell
curl -v https://$HOST/api/v1/status 
```

The response you should get is a **401 Authentication Required** similar to below:

```shell
> GET /api/v1/status HTTP/2
> Host: localhost:80
> User-Agent: curl/7.58.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 401 
< content-type: application/json
< content-length: 54
< date: Thu, 28 Nov 2019 16:58:23 GMT
< 
{"status":"FAIL","message":"Authentication Required"}
```

**If this is the response you get, it means the authentication module is working.**

Let’s try to get a response using the Node certificate we created. Execute the same command, but now pass Node certificate and key:

```shell
curl -v https://$CONTROLLER_ADDRESS/api/v1/status --cert /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-crt.pem --key /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-key.pem
```

If everthing is configured correctly, you should see something like this (I used 127.0.0.1 to setup this example):

```shell
*   Trying 127.0.0.1...

. . .

> GET /api/v1/status HTTP/2
> Host: 127.0.0.1:80
> User-Agent: curl/7.64.1
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS == 250)!
< HTTP/2 200 
< cache-control: no-store
< content-type: application/json
< content-length: 184
< date: Sun, 12 Apr 2020 04:26:13 GMT
< 
{"status":"OK","message":"","body":{"status":"Running","version":"1.7.0-4e6617d3","registry_address":"https://127.0.0.1:8089","registry_status":"Running","license":"enterprise plus"}}
* Connection #0 to host 127.0.0.1 left intact
* Closing connection 0
```

## 6. Final Notes

- If you enabled AUTH for the registry, you'll need to ensure that you set the `ANKA_CLIENT_CERT` and `ANKA_CLIENT_CERT_KEY` in your controller config or else it won't be able to communicate with the registry.
  ```
  ANKA_CLIENT_CERT	(string)	(Certificate Authentication) The Controller will use this when making http requests, mainly to the Registry	
  ANKA_CLIENT_CERT_KEY	(string)	(Certificate Authentication) The Controller will use this when making http requests, mainly to the Registry
  ```
- You may notice that the Controller UI doesn't load or acts strangely. You will need to enable [Root Token Authentication]({{< relref "Anka Build Cloud/Advanced Security Features/token-authentication.md" >}}) to access the controller UI.
- If you get an invalid cert error from the Controller UI, make sure that you add the root CA you generated to your system keychain.
