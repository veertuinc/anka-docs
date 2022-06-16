---
date: 2019-12-12T00:00:00-00:00
title: "Certificate Authentication"
linkTitle: "Certificate Authentication"
weight: 2
description: >
  How to set up certificate based authentication.
---

{{< hint warning >}}
**This guide requires an Anka Enterprise (or higher) license.**
{{< /hint >}}

There are several different ways you can enable Certificate authentication:

1. With the combined (controller + registry) native macOS package: You'll edit the `/usr/local/bin/anka-controllerd`, enable TLS/HTTPS (required), and then enable certificate authentication.
2. With the docker package: You'll edit the `docker-compose.yml`, enable TLS/HTTPS, and then enable certificate authentication.
3. With either the controller or registry standalone packages: You'll edit the proper config files, enable TLS/HTTPS, and then enable certification authentication.

## Requirements

1. A Root CA certificate. For more information about CAs, see https://en.wikipedia.org/wiki/Certificate_authority. Usually provided by your organization or where you obtain your certificate signing. We will generate a self-signed one in this guide and refer to this as **anka-ca-crt.pem** and **anka-ca-key.pem**.
2. Certificate[s] (signed with the Root CA) for the Anka Build Cloud Controller & Registry.
3. Certificate[s] (signed with the Root CA) for your Anka Build Nodes so they can connect/authenticate with the Anka Build Cloud Controller & Registry.

{{< hint warning >}}
If bringing your own certs, not password protected ("encrypted") (use `openssl rsa -in <encrypted_private.key> -out <decrypted_private.key>` to decrypt).
{{< /hint >}}

---

## 1. Create a self-signed Root CA certificate

If you don’t have a Root CA yet, you can create it with openssl:

```shell
cd ~
openssl req -new -nodes -x509 -days 365 -keyout anka-ca-key.pem -out anka-ca-crt.pem \
  -subj "/O=MyGroup/OU=MyOrgUnit/CN=MyUser"
```

{{< hint info >}}
You can add the Root CA to the System keychain so the Root CA is trusted and you can avoid warnings when you go to access the Controller UI.
```bash
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain anka-ca-crt.pem
```
{{< /hint >}}

## 2. Configuring TLS for Controller & Registry

{{< hint warning >}}
- TLS/HTTPS is required for Certificate Authentication.
- Your HTTPS/TLS cert can be from any root CA and does not need to be from the same CA as your authentication certs.
{{< /hint >}}

{{< include file="_partials/Anka Build Cloud/advanced-security-features/tls.md" >}}

## 3. Creating self-signed Node Certificates

The Controller's authentication module uses the Root CA (anka-ca-crt.pem) to authenticate any incoming requests. When the Node sends the requests to the Controller, it will present its certificates. Those certificates must have been generated from the Root CA and also, if using Enterprise Plus, have the necessary permissions.

You can use the following openssl commands to create Node certificates using the Root CA:

```shell
openssl genrsa -out node-$NODE_NAME-key.pem 4096
openssl req -new -sha256 -key node-$NODE_NAME-key.pem -out node-$NODE_NAME-csr.pem \
  -subj "/O=$ORGANIZATION/OU=$ORG_UNIT/CN=$NODE_NAME"
openssl x509 -req -days 365 -sha256 -in node-$NODE_NAME-csr.pem -CA anka-ca-crt.pem -CAkey anka-ca-key.pem \
  -CAcreateserial -out node-$NODE_NAME-crt.pem
```

## 4. Configuring the Controller & Registry to enable authentication

In addition to the node certificates, the controller itself makes API calls to the Registry (you've enabled registry auth, right?) to get templates, etc, and will need a client cert to communicate with it. This is where `ANKA_CA_CERT` comes in as it's used to validate the incoming requests.

### MacOS combined Controller & Registry package

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
The `ANKA_CA_CERT` is the authority that is used to validate the Anka Node Agent (`ankacluster join`) certs.
{{< /hint >}}

{{< hint warning >}}
**Until you have an Enterprise licensed Node joined to the Controller, it won't enable authentication for the Controller.**
{{< /hint >}}

{{< hint warning >}}
If you're connecting the Anka CLI with the HTTPS Registry URL, you can use the Node certificates: `anka registry --cert /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-crt.pem --key /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-key.pem add $REGISTRY_NAME https://$REGISTRY_ADDRESS:8089`
{{< /hint >}}

## 5. Testing & Joining your Node to the Controller with the Node certificate

Copy both the Node certificates (node-$NODE_NAME-crt.pem, node-$NODE_NAME-key.pem) and the anka-ca-crt.pem to the host/node you wish to join.

### Testing

Restart your Controller & Registry and then test the status endpoint with curl:

```shell
curl --insecure -v https://$HOST/api/v1/status 
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
curl --insecure -v https://$CONTROLLER_ADDRESS/api/v1/status --cert /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-crt.pem --key /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-key.pem
```

If everything is configured correctly, you should see something like this (I used 127.0.0.1 to setup this example):

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

### Joining

{{< hint warning >}}
If you previously joined your Nodes to the Controller, you'll want to `sudo ankacluster disjoin` on each before proceeding (if it hangs, use `ps aux | grep anka_agent | awk '{print $2}' | xargs kill -9` and try disjoin again).
{{< /hint >}}

{{< hint info >}}
Note: Certificates are cached, so if you update/renew them, you need to either:
1. disjoin and re-join them to the controller, issue `sudo pkill -9 anka_agent` on each node to restart the agent
2. or, issue a `<controller>/v1/node/update` PUT to the controller API to forcefully update all nodes.
{{< /hint >}}

{{< hint info >}}
If you're using a signed certificate for the controller dashboard, but self-signed certificates for your nodes and CI tools, you'll need to specify the `--cacert` for `ankacluster join` and `anka registry add` commands and point it to the signed CA certificate. You'll usually see `SSLError: ("bad handshake: Error([('SSL routines', 'tls_process_server_certificate', 'certificate verify failed')],)",)` if the wrong CA is being used.
{{< /hint >}}

Then, use the `ankacluster` command to connect it to the Controller with:

```shell
sudo ankacluster join https://$CONTROLLER_ADDRESS --skip-tls-verification \
  --cert /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-crt.pem --cert-key /Users/$USER_WHERE_CERTS_ARE/node-$NODE_NAME-key.pem
Testing connection to Controller...: OK
Testing connection to registry….: OK
Ok
Cluster join success
```

{{< hint info >}}
The `--skip-tls-verification` is only necessary if using a self-signed cert. Please avoid using `--skip-tls-verification` AND the `--cacert`.
{{< /hint >}}

---

## Accessing the Controller UI

Once Cert Auth as been enabled, loading your Controller UI will show `Controller Not Connected`. This is because the Controller is fully protected. In order to access the UI, you can set up your browser to use client certificates to access the page. Alternatively, you can enable root token auth with `ANKA_ROOT_TOKEN` which must be set to a minimum of 10 characters. You can read more about it [here]({{< relref "Anka Build Cloud/Advanced Security Features/token-authentication.md#protecting-your-cloud-with-rta-root-token-auth" >}}).

---

## Enabling Authorization (User/Group Permissions) (Enterprise Plus Only)

By default, `ANKA_ENABLE_AUTH` will not use authorization/permissions and allow any certs or users to connect to all API endpoints and pages in the UI. In order to enable Authorization, you will need to include specific ENVs in your config:

- `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` works for both combined and standalone (docker) packages.
- `ANKA_ENABLE_REGISTRY_AUTHORIZATION` is for the combined (controller + registry in one binary) package only.
- `ANKA_ENABLE_AUTHORIZATION` is only for the standalone registry package.

When creating certificates, you'll want to specify CSR values using openssl's `-subj` option. For example, if we're going to generate a certificate so our Jenkins instance can access the Controller & Registry, you'll want to use something like this:

```shell
-subj "/O=MyOrgName/OU=$ORG_UNIT/CN=Jenkins"
```

{{< include file="_partials/Anka Build Cloud/_managing-permissions.md" >}}

## 6. Final Notes

- If you enabled AUTH for the registry, you'll need to ensure that you set the `ANKA_CLIENT_CERT` and `ANKA_CLIENT_CERT_KEY` in your controller config or else it won't be able to communicate with the registry.
  ```
  ANKA_CLIENT_CERT	(string)	(Certificate Authentication) The Controller will use this when making http requests, mainly to the Registry	
  ANKA_CLIENT_CERT_KEY	(string)	(Certificate Authentication) The Controller will use this when making http requests, mainly to the Registry
  ```
- You may notice that the Controller UI doesn't load or acts strangely. You will need to enable [Root Token Authentication]({{< relref "Anka Build Cloud/Advanced Security Features/token-authentication.md" >}}) to access the controller UI.
- If you get an invalid cert error from the Controller UI, make sure that you add the root CA you generated to your system keychain.
