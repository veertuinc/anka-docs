---
date: 2019-07-03T22:24:47-05:00
title: "UAK / TAP Authentication"
weight: 3
description: >
  How to protect your Controller UI, API, and Registry API with a Root Token and/or User API keys.
---

{{< hint warning >}}
###### Important
- This feature requires either `Enterprise` or `Enterprise Plus`. There are several license specific differences that should be noted before you begin:
  - **Enterprise:** By default, any secrets you generate and use (root or user) always have full access to the API.
  - **Enterprise Plus:** By default, _only_ the root token (RTA) has full access to the API. User tokens _must be_ created with a group attached and permissions added for the group.
- Your Nodes will lose connection until you join them using the new credential.
{{< /hint >}}

Starting in 1.19.0 of the Anka Build Cloud, you can generate a UAK which is then used to request a temporary auth session and token for a client. These session tokens allow access to the API to perform various tasks.

---

## How to configure

1. Follow the same instruction from the above root token section, but also include `ANKA_ENABLE_API_KEYS` set to `true`.

2. Use the Controller's User API Keys UI panel or API to generate a user key for [the controller]({{< relref "anka-build-cloud/working-with-controller-and-API.md#user-key-management" >}}) and also [the registry]({{< relref "anka-build-cloud/working-with-registry-and-API.md#user-key-management" >}}). **KEEP THESE SECRET.**

![management ui for uak]({{< siteurl >}}images/anka-build-cloud/advanced-security-features/uak-management-ui-1.png)

3. You can now use the key and ID to communicate with the Controller and/or Registry through the `ankacluster join` command, `anka registry` (only Anka version 3.1 or higher), or in your client (through a TAP session/token) to the APIs.

{{< hint warning >}}
Each UAK can have one or more TAP generated sessions. This means you can generate a single UAK for a single piece of software which is deployed multiple times, and each software instance will get its own TAP generated session, independent from the others, but using the same key. An example of this is having a single UAK for all of your Anka Nodes to use when joining.
{{< /hint >}}

---

## Joining Nodes

Once you have the UAK key generated from Step 2 (above), you can use it to join the Anka Node.

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

{{< hint info >}} Instead of passing the private key as a string (`--api-key-string`), you can specify the path to the key file with `--api-key-file`.{{< /hint >}}

---

## Token Authentication Protocol (TAP)

This communication protocol is for user authentication using asymmetric encryption. You'll use this if you plan on making calls using an Authorization: bearer header to the API. The API is separate from the usual /api/v1/.

{{< hint warning >}}
- OpenSSL >= 3.x is required (macOS defaults to 2.x). The example below will install openssl 3 with brew and set it in the PATH ENV.
- We highly recommended enabling HTTPS.
- Base64 uses safe url encoding.
- Key length is 2048, Hashing algorithm SHA256, OAEP padding.
- Private key format is PEM PKCS#1.
- Public key format is PKIX, ASN.1 DER.
{{< /hint >}}

### How it works

1. You've generated a UAK and it's stored on the Controller and/or Registry with some unique identifier.
2. Your client sends the first phase of the authentication: `POST /tap/v1/hand -d '{"id": "<API-KEY-USER-ID>" }'` (doesn't need auth to communicate with).
3. The server generates a random string, encrypts it with the client’s public key that is has stored, encodes it in base64, and passes it back to the requesting client in the response body.
4. Your client then decodes and decrypts the response using the UAK private key it has available locally, and then sends the second phase of the authentication with the decoded string as `SECRET-STRING`: `POST /tap/v1/shake -d '{"id": "<API-KEY-USER-ID>", "secret": "<SECRET-STRING>" }'`

  {{< hint warning >}}
  By default, the secret is valid for 3 minutes. Also, you can only call /tap/v1/shake once for a secret after which it will become invalid.
  {{< /hint >}}

5. The server then successfully authenticates the client and returns the following object (base64 encoded) in the response body:
`{ "id": <API-KEY-USER-ID>, "data": <GENERIC-OBJECT> }`

6. You then take the contents of `data` in the response, base64 it, and use it in your Authorization: Bearer header for API calls.

The `data` object should look like

```javascript
{
  "userName": <USER-IDENTIFIER>
  "sessionId": <SESSION-ID>
  "token": <SESSION-TOKEN>
}
```

An example of the entire flow using BASH and CURL:

```bash
❯ brew install openssl
❯ export PATH="/usr/local/opt/openssl@3/bin:$PATH"

❯ ls nathan-*pem
nathan-key.pem nathan-pub.pem

❯ echo -n $(curl -s http://anka.controller:8090/tap/v1/hand -d '{"id": "nathan"}') | base64 -d > to_decrypt

# You have 10 seconds after obtaining the /hand token to shake. Otherwise, you will see an error thrown due to expiration.

❯ openssl pkeyutl -decrypt -inkey nathan-key.pem -in to_decrypt -out decrypted -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256

❯ cat decrypted
59A8Zb8XxtsBNpJ-1KYFKOzQfv8

❯ curl -s http://anka.controller:8090/tap/v1/shake -d "{\"id\": \"nathan\", \"secret\": \"$(cat decrypted)\" }"
{"id":"nathan","data":{"userName":"nathan","token":"XtACTPdOC_vF03s75EWSGunMGCDiU2aBIe97Pai0ruRDjhNpPZqg2w","sessionId":"34c06a2b-141a-434a-5313-a06788f20957"}}

❯ echo '{"id":"nathan","data":{"userName":"nathan","token":"6Z8JVhdo8MNUUHmFVy0bgjoSuiVJAsNYsqTdpqklqYv7j7xlJo6c2w","sessionId":"6dc7bd5e-a4b4-4c99-4b2e-c299fc101dc0"}}' | jq -r '.data' | base64
ewogICJ1c2VyTmFtZSI6ICJuYXRoYW4iLAogICJ0b2tlbiI6ICI2WjhKVmhkbzhNTlVVSG1GVnkwYmdqb1N1aVZKQXNOWXNxVGRwcWtscVl2N2o3eGxKbzZjMnciLAogICJzZXNzaW9uSWQiOiAiNmRjN2JkNWUtYTRiNC00Yzk5LTRiMmUtYzI5OWZjMTAxZGMwIgp9Cg==

❯ curl -s http://anka.controller:8090/api/v1/status
{"status":"FAIL","message":"Authentication Required"}

❯ curl -sH "Authorization: Bearer ewogICJ1c2VyTmFtZSI6ICJuYXRoYW4iLAogICJ0b2tlbiI6ICI2WjhKVmhkbzhNTlVVSG1GVnkwYmdqb1N1aVZKQXNOWXNxVGRwcWtscVl2N2o3eGxKbzZjMnciLAogICJzZXNzaW9uSWQiOiAiNmRjN2JkNWUtYTRiNC00Yzk5LTRiMmUtYzI5OWZjMTAxZGMwIgp9Cg==" http://anka.controller:8090/api/v1/status
{"status":"OK","message":"","body":{"status":"Running","version":"1.25.0-b2a027a4","registry_address":"http://anka.registry:8089","registry_status":"Running","license":"enterprise plus"}}
```

{{< hint warning >}}
You have 10 seconds after obtaining the /hand token to shake. Otherwise, you will see an error thrown due to expiration.
{{< /hint >}}

{{< hint warning >}}
By default, the TTL for keys is 5 minutes. You can modify this with the `ANKA_API_KEYS_SESSION_TTL`, set in the Controller and Registry configs. The TTL however will not cause long running requests, like downloads, to be interrupted.
{{< /hint >}}

---

## Managing User/Group Permissions (Authorization)

{{< hint warning >}}
###### Important
- You can set groups for each api key or use the API Key ID as the "username" and create individual permissions for them.
- Note for Enterprise Plus customers using OpenID and UAK: Typically, Authorization is enabled with the `ANKA_ENABLE_CONTROLLER_AUTHORIZATION` and other similar ENVs. However, when using your Ent+ license with OpenID and UAK, you will **always** need to add permissions for the groups claim you set in your OpenID configuration. Otherwise, you'll see a blank Controller UI and API requests will fail.
{{< /hint >}}

{{< include file="_partials/anka-build-cloud/advanced-security-features/authorization.md" >}}
