---
date: 2022-12-20T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.31.0"
---

### Ability to set custom scopes with `ANKA_OIDC_SCOPES` ENV

Requested scopes for OIDC are being sent by the Client (UI), and right now simply correlate to the claim name (“groups” and “name”). In most situations this is fine, however, some users need to specify different scopes. This can be done with the `ANKA_OIDC_SCOPES` ENV, specify a comma separated value for each desired scope.

### Ability to call group claims from userinfo endpoint with `ANKA_OIDC_USER_INFO` ENV

Sometimes IDP tokens won’t return groups claims and will only be returned as part of `userinfo` endpoint. We now allow enabling `ANKA_OIDC_USER_INFO` which will get claims from `userinfo` instead of the token. We have also added `ANKA_OIDC_CACHE_TTL` which allows control over how long we cache the returned group list from the endpoint (and all other OIDC requests) as calls to this endpoint can have a performance impact.
