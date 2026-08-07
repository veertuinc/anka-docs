---
date: 2019-12-12T00:00:00-00:00
title: "Configuring OpenID Connect (OIDC) / SSO based authentication"
linkTitle: "OpenID Connect (OIDC) / SSO Authentication"
weight: 5
description: >
  How to set up OIDC / SSO for the Anka Build Cloud Controller UI.
---

Many organizations and developers are already familiar with OpenID Connect (OIDC). OIDC is a layer that sits on top of OAuth 2.0 and performs the authorization necessary to access protected resources, such as the Anka Build Cloud Controller. This page walks through what you need to know to set it up and protect your Controller Dashboard/UI, with a full Okta example.

{{< hint warning >}}
###### Important
- Requires an Anka Enterprise Plus license.
- It currently only protects the UI/Dashboard and is not available for API or others types of protection.
- Your Nodes will lose connection until you join them using the new credential.
- We currently only support `Code/Explicit Flow`.
{{< /hint >}}

{{< hint error >}}
**Okta must support custom authorization servers. Please check with your Okta admin to ensure this is possible.**
{{< /hint >}}

---

## Configure Anka Controller OIDC with Okta

### 1. Gather the required information

Before you configure Okta, determine the externally visible Controller URL.

Production example:

```text
https://controller.example.com
```

Local test example:

```text
http://anka.controller.local
```

The Controller constructs its callback URL from the browser-visible origin:

```text
<CONTROLLER_ORIGIN>/oidc/v1/callback
```

Therefore, the examples above use:

```text
https://controller.example.com/oidc/v1/callback
http://anka.controller.local/oidc/v1/callback
```

The scheme, hostname, port, and path must match exactly. If users can access the Controller through multiple origins, register every corresponding callback URI.

Use HTTPS in production.

### 2. Create the Okta application

In the Okta Admin Console:

1. Open **Applications → Applications**.
2. Select **Create App Integration**.
3. Select **OIDC - OpenID Connect**.
4. Select **Web Application**.
5. Configure:

   - **App integration name:** `Anka Controller`
   - **Grant type:** `Authorization Code`
   - **Sign-in redirect URI:**

     ```text
     https://controller.example.com/oidc/v1/callback
     ```

   - **Sign-out redirect URI:**

     ```text
     https://controller.example.com
     ```

6. Under **Controlled access**, choose which users may use the integration.

For production, assigning specific Okta groups is recommended. If **Allow everyone in your organization to access** is selected, every Okta user can authenticate, although Controller permissions still determine what they can do.

Save the integration and securely record:

- Client ID
- Client secret

Okta’s official guide covers the same OIDC web-application and redirect model: [Sign users in to a web app using the redirect model](https://developer.okta.com/docs/guides/sign-into-web-app-redirect/main/).

### 3. Configure the authorization server

Use an Okta custom authorization server. Most Okta organizations provide one named `default`.

Open:

**Security → API → Authorization Servers → default**

The corresponding Controller provider URL is:

```text
https://<OKTA_DOMAIN>/oauth2/default
```

Example:

```text
https://example.okta.com/oauth2/default
```

Do not use only `https://example.okta.com` when the claims were configured on the `default` custom authorization server.

{{< hint info >}}
Don't know what URL to use for your provider? The provider URL + `/.well-known/openid-configuration` must lead to the issuer's OIDC config.
{{< /hint >}}

#### Add an access policy

Open the authorization server’s **Access Policies** tab.

1. Select **Add Policy**.
2. Give it a name such as `Anka Controller`.
3. Assign it to the Anka Controller client application.

Assigning the policy only to the Anka client is preferable to using **All Clients**.

Add a rule with:

- **Rule name:** `Allow Anka Controller login`
- **Grant type:** Authorization Code
- **User:** Any user assigned to the app
- **Scopes:** Any scopes, or at minimum `openid`, `profile`, and `email`
- **Token lifetime:** Use the organization’s security policy

An access policy and at least one matching rule are required. Some newer Okta organizations don’t create a default policy automatically. See [Okta access policies](https://help.okta.com/en-us/content/topics/security/api-config-access-policies.htm).

### 4. Add the groups claim

The Controller expects a claim named `groups` by default. It uses that claim for authorization.

Open:

**Security → API → Authorization Servers → default → Claims**

Select **Add Claim** and configure:

- **Name:** `groups`
- **Include in token type:** `ID Token`
- **ID token inclusion:** `Always`
- **Value type:** `Groups`
- **Filter:** `Matches regex`
- **Value:** `.*`
- **Include in:** Any scope

For initial testing, `.*` is convenient because it includes every matching Okta group. For production, restrict the expression to Controller groups, for example:

```text
^anka-.*
```

Then use Controller group names such as:

```text
anka-users
anka-operators
anka-administrators
```

{{< hint info >}}
Okta limits the number of groups that can be included in a groups claim, so a restricted filter is safer for organizations with many groups. If Okta returns too many groups, login can succeed in Okta and then return you to the Controller login page. See [Customize tokens with a groups claim](https://developer.okta.com/docs/guides/customize-tokens-groups-claim/main/).
{{< /hint >}}

{{< hint warning >}}
Configure the claim in the **ID token**, not only the access token. By default, the Controller extracts its user and group claims from the ID token.
{{< /hint >}}

### 5. Configure MFA

MFA is controlled by Okta’s authentication policy, independently of the OIDC application configuration.

Open the Anka Controller application and select its **Sign On** or **Authentication Policy** settings. Assign a policy appropriate for the organization, such as:

```text
Password + another factor
```

Users must enroll in an allowed factor, such as Okta Verify, before they can complete login.

### 6. Create and assign Okta groups

In Okta:

1. Open **Directory → Groups**.
2. Create the desired groups.
3. Use lowercase names to make matching predictable, for example:

   ```text
   team1
   team2
   anka-administrators
   ```

4. Open each group and select **Assign people**.
5. Add the appropriate users.
6. If application access is restricted by assignment, also assign these groups to the Anka Controller application.

Okta group management is documented under [Groups](https://help.okta.com/en-us/Content/Topics/users-groups-profiles/usgp-about-groups.htm).

### 7. Configure the Controller

Add the following environment variables to the Controller service:

```bash
ANKA_ENABLE_AUTH=true
ANKA_ENABLE_CONTROLLER_AUTHORIZATION=true

ANKA_ROOT_TOKEN=<STRONG-ADMIN-TOKEN>

ANKA_OIDC_PROVIDER_URL=https://<OKTA_DOMAIN>/oauth2/default
ANKA_OIDC_CLIENT_ID=<OKTA-CLIENT-ID>
ANKA_OIDC_CLIENT_SECRET=<OKTA-CLIENT-SECRET>
ANKA_OIDC_DISPLAY_NAME="Company Okta"

ANKA_OIDC_USERNAME_CLAIM=name
ANKA_OIDC_GROUPS_CLAIM=groups
```

For the local test domain:

```bash
ANKA_OIDC_PROVIDER_URL=https://<OKTA_DOMAIN>/oauth2/default
```

The provider URL remains the Okta URL; `anka.controller.local` is used only as the Controller origin and callback host.

Optional settings:

```bash
ANKA_OIDC_USER_INFO=false
```

Keep this disabled when `name`, `email`, and `groups` are present in the ID token. Enable it only when the identity provider intentionally returns the required claims through its UserInfo endpoint.

{{< hint warning >}}
**The OIDC ENVs must be set for both the Controller and Registry services.**
{{< /hint >}}

Restart the Controller after changing its environment.

Here is an example `docker-compose` config for Okta:

```docker
  anka-controller:
    container_name: anka-controller
    build:
       context: .
       dockerfile: anka-controller.docker
    ports:
       - "8090:80"
    volumes:
       - /Users/myUserName:/mnt/cert
    depends_on:
       - etcd
       - anka-registry
    restart: always
    environment:
      ANKA_ANKA_REGISTRY: "https://anka.registry:8089"
      ANKA_USE_HTTPS: "true"
      ANKA_SKIP_TLS_VERIFICATION: "false"
      ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      ANKA_ENABLE_AUTH: "true"
      ANKA_ENABLE_CONTROLLER_AUTHORIZATION: "true"
      ANKA_ROOT_TOKEN: "1111111111"
      ANKA_OIDC_DISPLAY_NAME: "Company Okta"
      ANKA_OIDC_PROVIDER_URL: "https://dev-1234567.okta.com/oauth2/default"
      ANKA_OIDC_CLIENT_ID: "0oa7a07mc0kQxyfrus11"
      ANKA_OIDC_CLIENT_SECRET: "aHWQYCbH0mTYwLwwIfBvT-JWotYQAR8HAn7glnSB"
      ANKA_OIDC_USERNAME_CLAIM: "name"
      ANKA_OIDC_GROUPS_CLAIM: "groups"
      ANKA_OIDC_USER_INFO: "false"

  anka-registry:
    container_name: anka-registry
    build:
        context: .
        dockerfile: anka-registry.docker
    ports:
        - "8089:8089"
    restart: always
    volumes:
      - "/Library/Application Support/Veertu/Anka/registry:/mnt/vol"
      - /Users/myUser/:/mnt/cert
    environment:
      ANKA_USE_HTTPS: "true"
      ANKA_SKIP_TLS_VERIFICATION: "false"
      ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      ANKA_ENABLE_AUTH: "true"
      ANKA_OIDC_DISPLAY_NAME: "Company Okta"
      ANKA_OIDC_PROVIDER_URL: "https://dev-1234567.okta.com/oauth2/default"
      ANKA_OIDC_CLIENT_ID: "0oa7a07mc0kQxyfrus11"
      ANKA_OIDC_CLIENT_SECRET: "aHWQYCbH0mTYwLwwIfBvT-JWotYQAR8HAn7glnSB"
```

After that, run `docker-compose down -t 50 && docker-compose up -d` and open the Controller at its HTTPS URL. You should see a Log In box with two options: `Login with Company Okta` and `Login with superuser`.

![OpenID Login Buttons]({{< siteurl >}}images/openid/login.png)

### 8. Create matching Controller permission groups

Authentication and authorization are separate:

- Okta authenticates the user and sends group names.
- Anka Controller maps those names to Controller permission groups.
- A user whose Okta groups do not match any Controller permission group can log in but may see an empty dashboard.

Log into the Controller as the superuser:

- **Username:** `root`
- **Password:** value of `ANKA_ROOT_TOKEN`

Then:

1. Open **Permissions**.
2. Select the **Controller** component.
3. Create a group whose name exactly matches the Okta group.
4. Assign the required permissions.
5. Save the group.

Example:

```text
Okta group:       team1
Controller group: team1
```

Group matching is case-sensitive in practice. Prefer lowercase group names on both sides.

If Registry authorization is enabled, select the **Registry** component and configure the same group’s Registry permissions as well.

If resource management is enabled, use the group’s **Resources** tab to limit access to specific templates, nodes, or other resources.

For full detail on permission groups, resources, and resource permissions, see [Managing User/Group Permissions (Authorization)](#managing-usergroup-permissions-authorization).

### 9. Test the integration

Use a private/incognito browser so an existing Okta administrator session does not interfere.

1. Open the externally visible Controller URL.
2. Select **Login with Company Okta**.
3. Sign in as a non-administrator test user.
4. Complete MFA if required.
5. Confirm that the user can open the pages permitted by their Controller group.

After changing Okta group membership, the user must log out and log back in. Existing tokens retain the old group list until a new OIDC login completes.

A successful token should contain claims similar to:

```json
{
  "name": "Team1 User1",
  "email": "team1user1@example.com",
  "groups": [
    "Everyone",
    "team1"
  ]
}
```

Do not paste production ID tokens into public JWT-decoding websites. Use Okta’s **Token Preview** or an approved internal tool.

---

## Troubleshooting

### Login succeeds, but the dashboard is blank

Most likely cause:

```text
groups: ["Everyone"]
```

with no matching `Everyone` Controller permission group.

Verify that:

- The user belongs to the intended Okta group.
- The groups claim is included in the ID token.
- The Controller group has exactly the same name.
- Permissions are assigned to that Controller group.
- The user logged out and back in after the membership change.

### “User is not assigned to the client application”

The user or one of their groups has not been assigned to the Okta application, or the application’s controlled-access setting is too restrictive.

### Okta returns `access_denied`

Check the custom authorization server:

- An access policy exists.
- The policy applies to the Anka Controller client.
- A matching rule exists.
- Authorization Code is permitted.
- The user satisfies the rule.

### Redirect URI mismatch

Confirm the exact callback URI:

```text
https://controller.example.com/oidc/v1/callback
```

Check the scheme, hostname, port, and path. The Controller uses the origin visible in the user’s browser, including a nonstandard port if present.

### Controller reports a missing `groups` claim

The claim may have been:

- Added only to the access token
- Added to a different authorization server
- Configured with a filter that matches no groups
- Configured under a different claim name

Either name it `groups` or set:

```bash
ANKA_OIDC_GROUPS_CLAIM=<CUSTOM-CLAIM-NAME>
```

### User name claim is missing

The default username claim is `name`. If the provider doesn’t supply it, use another ID-token claim:

```bash
ANKA_OIDC_USERNAME_CLAIM=email
```

### Security precautions

- Store the OIDC client secret in a secrets manager.
- Use a strong, unique `ANKA_ROOT_TOKEN`.
- Use HTTPS for all production Controller URLs.
- Restrict Okta app assignments and group-claim filters.
- Avoid high Controller verbosity in production: verbose OIDC diagnostics may log tokens or claims.
- Rotate the Okta client secret if it is exposed, then update the Controller and restart it.

---

## Managing User/Group Permissions (Authorization)

After Okta returns group names in the ID token, add those exact names as Controller permission groups and assign permissions. This gives users in the matching Okta group access to the Controller UI and API actions you allow.

{{< include file="_partials/anka-build-cloud/advanced-security-features/authorization.md" >}}

Once the permissions are set, log out of the superuser account and choose **Login with Company Okta**. Okta authenticates the user, then the Controller applies the matching permission group.

---

## FAQ

### How do I confirm the correct `ANKA_OIDC_PROVIDER_URL`?

Open:

```text
<ANKA_OIDC_PROVIDER_URL>/.well-known/openid-configuration
```

That URL must return the issuer’s OIDC discovery document. For Okta’s `default` custom authorization server, the provider URL is usually:

```text
https://<OKTA_DOMAIN>/oauth2/default
```

So the discovery document is:

```text
https://<OKTA_DOMAIN>/oauth2/default/.well-known/openid-configuration
```

If that page is missing or returns an error, the provider URL is wrong for the authorization server that holds your claims.

In the discovery JSON, check `scopes_supported`. For Okta it often looks like:

```json
"scopes_supported": [
  "openid",
  "profile",
  "email",
  "groups"
]
```

### What scopes and claims does the Controller expect?

By default the Controller looks for the `openid`, `profile`, and `groups` scopes, and requires these claims:

- `name` (from `profile`)
- `groups`

Override the claim names with:

```bash
ANKA_OIDC_USERNAME_CLAIM=name
ANKA_OIDC_GROUPS_CLAIM=groups
```

Override the requested scopes with `ANKA_OIDC_SCOPES` only when your provider needs a different list. See the [Configuration Reference]({{< relref "anka-build-cloud/configuration-reference.md" >}}) for all `ANKA_OIDC_*` options.

The `groups` claim must be an array of strings. Each string must match a [Controller permission group](#8-create-matching-controller-permission-groups) name.

### Does the redirect URI need to be reachable from the public internet?

No. The redirect URI does not need to be public. It must match the hostname or IP (and port) that users type in the browser when they open the Controller.

Examples:

```text
https://controller.example.com/oidc/v1/callback
https://anka.controller:8090/oidc/v1/callback
http://anka.controller.local/oidc/v1/callback
```

### Which OAuth / OIDC flow is supported?

Only Authorization Code (also called Code / Explicit Flow). Implicit Flow is not supported.

OIDC currently protects the Controller UI/Dashboard only. It is not used for API or CLI authentication.

### Must OIDC settings be set on both Controller and Registry?

Yes. Set the OIDC environment variables on both services. After you enable authentication, Nodes lose their connection until you join them again with the new credential.

### When should I enable `ANKA_OIDC_USER_INFO`?

Keep `ANKA_OIDC_USER_INFO=false` when `name`, `email`, and `groups` are already present in the ID token. That is the recommended Okta setup in this guide.

Enable UserInfo only when your identity provider returns the required claims from the UserInfo endpoint instead of the ID token. If the Controller cannot find groups, you may see:

```text
failed translating claims to user: no Groups claim in groups
```

In that case, either add the `groups` claim to the ID token, or enable UserInfo and configure the provider so UserInfo returns `groups`.

### Okta login succeeds, then the Controller returns me to the login page. Why?

Okta can return too many groups in the token. The Controller then fails to complete login and sends you back to the login page.

Limit the groups claim filter in Okta (for example `^anka-.*`) so the token includes only the groups you need. See [Add the groups claim](#4-add-the-groups-claim).

A blank dashboard after a successful login is a different problem: the user authenticated, but no Controller permission group matched. See [Login succeeds, but the dashboard is blank](#login-succeeds-but-the-dashboard-is-blank).

### How long do Controller UI sessions last?

UI sessions last as long as the OIDC Access Token lifetime. Shorten that lifetime in your provider if you need shorter Controller sessions.

### Can I use a provider other than Okta?

Yes. Most customers use providers such as Okta, CyberArk Idaptive, or similar OIDC servers. The Controller requirements stay the same:

- Authorization Code flow
- Redirect to `/oidc/v1/callback`
- `name` and `groups` claims (or your custom claim names)
- Matching Controller permission groups

Ask your identity team to map those requirements onto your company’s preferred tools.
