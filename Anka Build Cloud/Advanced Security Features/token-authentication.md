---
date: 2019-07-03T22:24:47-05:00
title: "Configuring Token Authentication"
linkTitle: "Token Authentication"
weight: 1
description: >
  How to protect your Controller UI, API, and Registry API with a Root Token and/or User API keys.
---

{{< hint warning >}} **This guide requires an Anka Enterprise or Enterprise Plus license.** {{< /hint >}}

Starting in 1.19.0 of the Anka Build Cloud, there are two options for securing the communication with the Build Cloud Controller & Registry:

1. Setting the Root Token, protecting the Controller API and UI.

2. Generating a user private key which is then used to request temporary session tokens by the client. These session tokens allow access to the API to perform various tasks.

However, there are several license specific differences that should be noted before you begin:

- **Enterprise:** By default, any tokens you generate (root or user) always have full access to the API.
- **Enterprise Plus:** By default, _only_ the root token has full access to the API. User tokens _must be_ created with a group attached and permissions added for the group.

{{< hint info >}}
We recommend disjoining all but one node. One node must stay joined to ensure the Build Cloud has the proper license attached. You can rejoin it later once you're configured and you've rejoined all of your other nodes.
{{< /hint >}}

---

## Protecting your cloud with RTA (Root Token Auth)

Enabling root token authentication is a simple process. The root user has what we call "superuser" (full) access to the controller, API (basic auth), etc.

### How to configure RTA
##### MacOS Package

Edit `/usr/local/bin/anka-controllerd` and add:

```bash
export ANKA_ENABLE_AUTH="true"
export ANKA_ROOT_TOKEN="{min10chartoken}"
```

{{< hint info >}}The macOS package has no way to set tokens or auth for either the controller or registry. It will be enabled for both if the ENV is set to true.{{< /hint >}}

{{< hint warning >}}**The root token must be at least 10 characters long.**{{< /hint >}}

##### Linux/Docker Package

{{< hint info >}}With our docker package, each service is split up into its own container. You can enable a root token for either the controller, registry, or both.{{< /hint >}}

Edit the `docker-compose.yml` and add both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` environment variables:

{{< highlight dockerfile "hl_lines=16 17" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "80:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
   restart: always
   environment:
     ANKA_ENABLE_AUTH: "true"
     ANKA_ROOT_TOKEN: "1111111111"
     # ANKA_ENABLE_API_KEYS="true"

anka-registry:
   build:
      context: .
      dockerfile: anka-registry.docker
   ports:
      - "8089:8089"
   . . .
   environment:
     ANKA_ENABLE_AUTH: "true"
     ANKA_ROOT_TOKEN: "1111111111"
     # ANKA_ENABLE_API_KEYS="true"
. . .

{{</ highlight >}}

### Testing RTA

If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear.

![root token login]({{< siteurl >}}images/anka-build-cloud/advanced-security-features/controller-root-token-login.png)

Enter the token you specified and ensure that it logs you in.

Finally, you can test the API using:

```bash
❯ curl -H "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/registry/status
{"status":"OK","body":{"status":"Running","version":"1.19.0-309d8150"},"message":""}
```

{{< hint warning >}}
Enabling RTA will block any access to the UI and API for Anka Nodes joined to the primary interface/port for the controller. Instead, you can [expose a queue only interface]({{< relref "Anka Build Cloud/configuration-reference.md#separate-queue-interface" >}} instead which can be used to join your nodes.
{{< /hint >}})

---

## Protecting your cloud with UAK (User API Keys)

### How to configure UAK

1. Follow the same instruction from the above root token section, but also include `ANKA_ENABLE_API_KEYS` set to `true`.

2. Use the API to generate a user key for [the controller]({{< relref "Anka Build Cloud/working-with-controller-and-API.md#user-key-management" >}}) and also [the registry]({{< relref "Anka Build Cloud/working-with-registry-and-API.md#user-key-management" >}}).

3. You can now use the key and ID to communicate with the Controller or Registry.

### Joining Nodes with your UAK

```bash
❯ sudo ankacluster join http://anka.controller:8090 --groups "gitlab-test-group-env" --reserve-space 10GB --api-key-id "nathan" --api-key-string "$ANKA_API_KEY_STRING"
I0920 15:31:15.008778   77147 factory.go:75] Default http client using API Key authentication
Testing connection to the controller...: Ok
Testing connection to the registry...: Ok
Success!
I0920 15:31:37.300568   77147 factory.go:75] Default http client using API Key authentication
Anka Cloud Cluster join success
```

{{< hint warning >}} At the moment the `ankacluster` command does not support ENVs. {{< /hint >}}

{{< hint info >}} Instead of passing the private key base64 as a string (`--api-key-string`), you can specify the path to the key file with `--api-key-file`.{{< /hint >}}

### Controller and Registry communication with your UAK

Should the Registry be protected by authentication and User API Keys, the Controller requires its own key for registry API calls. Once generated, you need to specify the `ANKA_API_KEY_ID` and `ANKA_API_KEY_STRING` (or file) ENVs described in the [Configuration Reference]({{< relref "Anka Build Cloud/configuration-reference.md#authentication-and-authorization" >}}).

---

## Token Authentication Protocol (TAP)

{{< hint info >}}
The following API is only useful if you'd like to build your own client to request and renew session tokens from UAKs.
{{< /hint >}}

{{< hint warning >}}
- Highly recommended to integrate with HTTPS
- Base64 uses safe url encoding
- Key length is 2048, Hashing algorithm SHA256, OAEP padding
- Private key format is PEM PKCS#1
- Public key format is PKIX, ASN.1 DER
{{< /hint >}}

This communication protocol is for user authentication using asymmetric encryption. The API is separate from the usual /api/v1/.

#### Authentication flow

1. A public key is stored on the server along with some unique identifier.
2. A client sends the first phase of the authentication: `POST /tap/v1/hand -d '{"id": "<USER-ID>" }'`
3. The server generates a random string, encrypts it with the client’s public key, encodes it in base64 and passes it back to the client in the response body
4. The client decodes and decrypts the response and sends the following the second phase of the authentication: `POST /tap/v1/shake -d '{"id": "<USER-ID>", "secret": "<SECRET-STRING>" }'`
5. The server successfully authenticates the client and returns the following object (base64 encoded) in the response body:
`{ "id": <USER-IDENTIFIER>, "data": <GENERIC-OBJECT> }`
6. The "data" field is a generic object, and can be customized for specific needs

#### Putting it all together

When an API Key is created, the public key and id are stored and later used in the TAP authentication protocol.

Upon a successful authentication with the TAP protocol, a session is created and the generic data object that is returned includes the following data:

```javascript
{
  "userName": <USER-IDENTIFIER>
  "sessionId": <SESSION-ID>
  "token": <SESSION-TOKEN>
}
```

This object, encoded in JSON and base64 is what the user supplies in the HTTP Authorization header, with the Bearer prefix, for future requests.

---

## Managing User/Group Permissions (Authorization)

{{< hint info >}}
Your `--api-key-id` is the `username`.
{{< /hint >}}

{{< include file="_partials/Anka Build Cloud/_managing-permissions.md" >}}
