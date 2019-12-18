---
date: 2019-07-03T22:24:47-05:00
title: "Advanced Security features"
linkTitle: "Advanced Security features"
weight: 9
description: >
  How to set up Authorization and SSO.
---



### Enterprise Plus Tier
Enterprise Tier supports SSO based authorization and authentication on top of TLS.

This tier supports 3 authentication methods and the concept of authorization.

1. Root token authentication
2. Client Certificate authentication
3. OpenId connect authentication(SSO)

**Authentication Method** | **Details**
--- | ---
Root token authentication | Superuser access to Anka controller’s dashboard
Certificate authentication | Access from Anka host machines(for registry), Nodes(both registry and controller), Plugins, Controller and registry API access
OpenId connect | Permission-based Anka Controller Portal Dashboard access


***Note***
It may be possible to use Openid connect for API access through service accounts depending on the Openid provider.

**How To** Anka Controller and Registry have built-in Certificate, Root token and Openid connect authentication support, configured through command-line arguments (or docker-compose.yml). 

#### Authorization
Each user of Anka Controller has two properties - username and groups.

Each group has a list of permissions that the users that belong to it are allowed to perform.

Groups can be configured using the admin UI, given that the current user has permissions to configure them or the user is authenticated using the root token (in which case the user is a “superuser”).

The authorization in the controller and registry performs checks against the groups the user belongs to.

For example:

1. Group “devops” has the “list_vms” permission
2. Group “admins” has the “start_vm” permission

User Bob is a member of  “devops” and “admins”. When user Bob logs in to the controller portal, Bob will be able to list vms and start vms.


The authorization in tlsProxy performs checks against a static list of groups and/or usernames given by command line parameters (or in docker-compose.yml).

For example:

Node mac_mini_1 is member of group “nodes”, tlsProxy is configured with groups “nodes”, “testers”, “admins”, Node mac_mini_1 is allowed to connect to tlsProxy


Another example :

Node mac_pro_1 is member of group “admins”, tlsProxy is configured  with groups “nodes” and names “mac_pro_1”, “mac_mini_1”, Node mac_pro_1 is allowed to connect to tlsProxy


#### Securing Anka VMs 
Enterprise Plus tier includes features to encrypt VMs at REST and control VM properties like networking, copy/paste, etc through VM policy.


