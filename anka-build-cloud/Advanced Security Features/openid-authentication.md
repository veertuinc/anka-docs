---
date: 2019-12-12T00:00:00-00:00
title: "Configuring OpenID Connect (OIDC) / SSO based authentication"
linkTitle: "OpenID Connect (OIDC) / SSO Authentication"
weight: 5
description: >
  How to set up OIDC / SSO for the Anka Build Cloud Controller UI.
---

Many organizations and developers are already familiar with OpenID Connect (OIDC). OIDC is a layer that sits on top of OAuth 2.0 and performs the authorization necessary to access protected resources, such as the Anka Build Cloud Controller. Let's walk through what you need to know to set it up and protect your Controller Dashboard/UI.

{{< hint warning >}}
###### Important
- Requires an Anka Enterprise Plus license.
- It currently only protects the UI/Dashboard and is not available for API or others types of protection.
- You must have at least one node with a `Enterprise` or higher license joined to the Controller for these features to work.
- Your Nodes will lose connection until you join them using the new credential.
- We currently only support `Code/Explicit Flow`.
{{< /hint >}}

---

## Usage Instructions

When using OIDC, you'll need an Authorization Provider or Server. Most of our customers use Providers like [Okta](https://www.okta.com/), [Cyberark's Idaptive](https://www.cyberark.com/products/workforce-identity/), and others. we won't get into the specifics for these tools as they often differ greatly. However we will go through several general things you need to know, regardless of provider.

### Required Changes

#### Anka Build Cloud Controller & Registry

1. Set `ANKA_OIDC_CLIENT_SECRET` to the Client Secret you generate at your provider application.
2. Set `ANKA_OIDC_PROVIDER_URL` to the appropriate provider URL for oauth2. For example, in Okta, I would set this to `https://dev-123456.okta.com/oauth2/default`.
  {{< hint info >}}
  Don't know what URL to use for your provider? The provider url + /.well-known/openid-configuration  must lead to the issuer's OIDC config.
  {{< /hint >}}
3. Set `ANKA_OIDC_CLIENT_ID` to the client ID for the application.


#### At Your Provider

1. Your provider config must allow a redirect to the Anka Controller at `/oidc/v1/callback`. The URL doesn't need to be public, but must match the hostname or IP (and port) you use locally. As an example, in Okta, you'll set `Sign-in redirect URIs` to `https://anka.controller:8090/oidc/v1/callback`.
2. The controller/your browser will request certain `scopes` from the provider. These `scopes` have `claims` attached. By default, we look for `openid`, `profile`, and `groups`.
3. Within the `scopes`, we look for `claims`. The following `claims` are required (by default): `name` (part of `profile`) & `groups`. These are changeable with `ANKA_OIDC_GROUPS_CLAIM` and `ANKA_OIDC_USERNAME_CLAIM` in your controller's config. In version 1.29.0, we will also look for `scopes` using the values of those ENVs.
4. Once the `scopes` are requested successfully, the data returned needs to be in a specific format (`id_token` & `token`). We make the request with `response_type` to ensure this.
5. The `groups` claim is expected to be an array of strings, each correlating to a [Controller permission group](#managing-usergroup-permissions-authorization).

---

Here is an example config showing what it looks like for a user with Okta:

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
      ANKA_ROOT_TOKEN: "1111111111"
      ANKA_OIDC_DISPLAY_NAME: "Okta SSO"
      ANKA_OIDC_PROVIDER_URL: "https://dev-1234567.okta.com/oauth2/default"
      ANKA_OIDC_CLIENT_ID: "0oa7a07mc0kQxyfrus11"
      ANKA_OIDC_CLIENT_SECRET: "aHWQYCbH0mTYwLwwIfBvT-JWotYQAR8HAn7glnSB"

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
      ANKA_OIDC_DISPLAY_NAME: "Okta SSO"
      ANKA_OIDC_PROVIDER_URL: "https://dev-1234567.okta.com/oauth2/default"
      ANKA_OIDC_CLIENT_ID: "0oa7a07mc0kQxyfrus11"
      ANKA_OIDC_CLIENT_SECRET: "aHWQYCbH0mTYwLwwIfBvT-JWotYQAR8HAn7glnSB"
```

{{< hint warning >}}
**The OIDC ENVs must be set for both services.**
{{< /hint >}}

After that, just `docker-compose down -t 50 && docker-compose up -d` and try accessing the Controller at its HTTPS URL or IP. If you did everything correctly (you enabled certificate authentication and joined your node right?), you should see a Log In box with two options: `Login with Okta SSO` and `Login with superuser`.

![OpenID Login Buttons]({{< siteurl >}}images/openid/login.png)

<!-- {{< hint info >}}
Not using Keycloak? No problem! For example in [CyberArk's Idaptive](https://www.cyberark.com/resources/videos/idaptive-product-overview), you need to create an `OpenID Connect` Web App, assign your user under Permissions, and then `setClaim('groups', 'sso-user-group');` under Tokens > Custom Logic. Once set up, you configure the controller to use `ANKA_OIDC_PROVIDER_URL="{OpenID Connect Issuer URL}"` and `export ANKA_OIDC_CLIENT_ID="{OpenID Connect Client ID}"`. At this point, you'd add `sso-user-group` under the Controller's `/admin/ui` permissions management panel (using the root token/user) and assign the proper permissions users of the web app can use. We recommend contacting your local IT team to help determining exactly what you'll need to configure this with your company's preferred tools.
{{< /hint >}} -->

<!-- 
In this guide, we will use **Keycloak** as our Authorization Server as it's fairly easy to run and setup. It will contain the realm, client ID, user, group, and anything else we will need for logging into the Anka Build Cloud Controller.

> This guide will be running the Anka Build Cloud and Keycloak on the same machine. It is meant to give you an idea of how to configure and is not recommended for production.

We will then log into the Anka Build Cloud Controller UI and use the `/admin/ui#/controllerGroups` page to create limited permissions for your groups.

## Setup Keycloak in Docker

### Run the docker container
```bash
docker run --rm -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin quay.io/keycloak/keycloak:latest
```

### Configure your Keycloak

1. Follow the instructions in https://www.keycloak.org/getting-started/getting-started-docker to set up your Keycloak:

  - I used `myrealm` as the Realm name.
  - I created user `nathan` with the password of `nathan` (turn off Temporary). I filled in my full name too.
  - When creating the Client, I set `anka` as the Client ID, clicked Save, then entered `https://anka.controller` (this is the URL for the controller I run) for the **Valid Redirect URIs**. I also set **Access Type** to **confidential** and enabled **Implicit Flow**.

2. Next, create a **Client Scope** named `groups`. Once created, under **Clients > anka > Client Scopes**, add the `groups` Client Scope (select it and then click **Add Selected**). Then, back under the `groups` **Client Scope**, **Mappers**, click **Add Builtin**, and choose `groups`, then **Add Selected**.

2. Under `Roles` add `anka-build-cloud-access`. 
    > The role is what matches with the group name in the Controller UI's Admin panel where you set specific access permissions for certain groups/users.

3. You can now create a group called `anka-build-cloud-access` and under **Role Mappings**, add the role: `anka-build-cloud-access`. Then, join it to the user you created.

At this point, you'll have Keycloak ready to use with your Anka Build Cloud Controller. Though, we need first to enable it.

## Enable OpenID in your Controller configuration

In order to enable OpenID, you'll need to modify your `docker-compose.yml` (if you're using our docker package) or the `/usr/local/bin/anka-controllerd` (if you're using the native Mac package).

> You can find a list of configuration options in the [Configuration Reference]({{< relref "anka-build-cloud/configuration-reference.md" >}}) by searching for `ANKA_OIDC`

Here is what your `docker-compose.yml` should look like for use with Keycloak:

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
      ANKA_ANKA_REGISRY: "https://anka.registry:8089"
      ANKA_USE_HTTPS: "true"
      ANKA_SKIP_TLS_VERIFICATION: "false"
      ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      ANKA_ENABLE_AUTH: "true"
      ANKA_ROOT_TOKEN: "1111111111"
      ANKA_OIDC_DISPLAY_NAME: "Keycloak"
      ANKA_OIDC_PROVIDER_URL: "http://host.docker.internal:8080/auth/realms/myrealm"
      ANKA_OIDC_CLIENT_ID: "anka"

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
      ANKA_OIDC_DISPLAY_NAME: "Keycloak"
      ANKA_OIDC_PROVIDER_URL: "http://host.docker.internal:8080/auth/realms/myrealm"
      ANKA_OIDC_CLIENT_ID: "anka"
```

{{< hint warning >}}
**The OIDC ENVs must be set for both services.**
{{< /hint >}}

After that, just `docker-compose down -t 50 && docker-compose up -d` and try accessing the Controller at its HTTPS URL or IP. If you did everything correctly (you enabled certificate authentication and joined your node right?), you should see a Log In box with two options: `Login with Keycloak` and `Login with superuser`

![OpenID Login Buttons]({{< siteurl >}}images/openid/login.png)

We first want to log in with superuser (the `ANKA_ROOT_TOKEN` defined above in the config).

Once logged in, you will see **Admin** on the left navigation

![Admin Navigation]({{< siteurl >}}images/openid/admin.png)

Under the **Admin** page, we want to add a **New Group**. **The Group Name will be the name of the group you created within Keycloak.** -->

---

## Managing User/Group Permissions (Authorization)

You can then add the exact `groups` claim name from your SSO provider (or other authorization server software) to the controller's permission management panel. This gives any users associated to the group in that cloud permission group the specific permissions to the API and even controller UI.

{{< include file="_partials/anka-build-cloud/advanced-security-features/authorization.md" >}}

Once you've added all of the proper permissions, you can now go back to the main Controller page and log out of the superuser. You can now choose **Login with Okta SSO**, which will redirect you to your Okta to have you log in with your user. You will then be taken to the Controller UI and be logged in as that user.

## Final Notes

- UI sessions will have the same length as the OIDC Access Token's lifetime. Your provider can set this value lower if needed.
