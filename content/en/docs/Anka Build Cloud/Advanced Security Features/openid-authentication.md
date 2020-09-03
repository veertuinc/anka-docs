---
date: 2019-12-12T00:00:00-00:00
title: "Configuring OpenID (SSO) based authentication"
linkTitle: "OpenID (SSO) Authentication"
weight: 3
description: >
  How to set up OpenID based authentication for your Anka Build Cloud Controller & Registry
---

> This guide and feature require an Anka Enterprise Plus license.

> Before you begin, you will need to setup [Certificate Authentication]({{< relref "docs/Anka Build Cloud/Advanced Security Features/certificate-authentication.md" >}}). This allows your Anka Nodes to join (and attach your Enterprise Plus license) to the Controller properly, since `ANKA_ENABLE_AUTH` will be turned on.

> You must have at least one node with a Enterprise or higher license joined to the Controller for these features to work.

Many organizations and developers are already familiar with OpenID Connect (OIDC). OIDC is a layer that sits on top of OAuth 2.0 and performs the authorization necessary to access protected resources, such as the Anka Build Cloud Controller.

When using OIDC, you'll need an Authorization Server. In this guide, we will use **Keycloak** as our Authorization Server as it's fairly easy to run and setup. It will contain the realm, client ID, user, group, and anything else we will need for logging into the Anka Build Cloud Controller.

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
  - When creating the Client, I set `anka` as the Client ID, clicked Save, then entered `https://anka.controller:8090` (this is the URL for the controller I run) for the **Valid Redirect URIs**. I also set **Access Type** to **confidential** and enabled **Implicit Flow**.

2. Next, create the `groups` **Client Scope**. Then, under **Clients > anka > Client Scopes**, add the `groups` Client Scope (select it and then click **Add Selected**). Then, back under the `groups` **Client Scope**, click **Add Builtin**, and choose `groups`, then **Add Selected**.

3. You can now create a group called `anka-controller-access` and then join it to the user you created.

At this point, you'll have Keycloak ready to use with your Anka Build Cloud Controller. Though, we need first to enable it.

## Enable OpenID in your Controller configuration

In order to enable OpenID, you'll need to modify your `docker-compose.yml` (if you're using our docker package) or the `/usr/local/bin/anka-controllerd` (if you're using the native Mac package).

> You can find a list of configuration options in the [Configuration Reference]({{< relref "docs/Anka Build Cloud/configuration-reference.md" >}}) by searching for `ANKA_OIDC`

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
      ANKA_REGISTRY_ADDR: "https://anka.registry:8091"
      ANKA_USE_HTTPS: "true"
      ANKA_SKIP_TLS_VERIFICATION: "false"
      ANKA_SERVER_CERT: "/mnt/cert/anka-controller-crt.pem"
      ANKA_SERVER_KEY: "/mnt/cert/anka-controller-key.pem"
      ANKA_CA_CERT: "/mnt/cert/anka-ca-crt.pem"
      ANKA_ENABLE_AUTH: "true"
      ANKA_ROOT_TOKEN: "1111111111"
      ANKA_OIDC_DISPLAY_NAME="Keycloak"
      ANKA_OIDC_PROVIDER_URL="http://host.docker.internal:8080/auth/realms/myrealm"
      ANKA_OIDC_CLIENT_ID="anka"

  anka-registry:
    container_name: anka-registry
    build:
        context: .
        dockerfile: anka-registry.docker
    ports:
        - "8091:8089"
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
```

After that, just `docker-compose down -t 50 && docker-compose up -d` and try accessing the Controller at its HTTPS URL or IP. If you did everything correctly (you enabled certificate authentication and joined your node right?), you should see a Log In box with two options: `Login with Keycloak` and `Login with superuser`

![OpenID Login Buttons](/images/openid/login.png)

We first want to log in with superuser (the `ANKA_ROOT_TOKEN` defined above in the config).

Once logged in, you will see **Admin** on the left navigation

![Admin Navigation](/images/openid/admin.png)

Under the **Admin** page, we want to add a **New Group**. **The Group Name will be the name of the group you created within Keycloak.** 

## Managing User/Group Permissions

{{< include file="shared/content/en/docs/Anka Build Cloud/Advanced Security Features/partials/_managing-permissions.md" >}}

Once you've added all of the proper permissions, you can now go back to the main Controller page and log out of the superuser. You can now choose **Login with Keycloak**, which will redirect you to your Keycloak to have you log in with the user you created earlier in this guide. You will then be taken to the Controller UI and be logged in as that user.