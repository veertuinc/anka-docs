---
date: 2021-10-25T01:00:00-00:00
title: "Anka Build Cloud Controller & Registry Version 1.18.0"
---

### Ability to use certs and username/password for etcd connections

In previous version of Anka Build Cloud Controller & Registry it was not possible to connect to an external etcd cluster using certificates or a username and password. We have added several ENVs for you to achieve both of these:

| Name | Type | Description | Default Value | ENV | 
| --- | :---: | --- | :---: | :---: | 
Enable Etcd Authentication| bool | Use TLS certificates for authentication with etcd server. **Must pass this for etcd authentication to work** | false | ANKA_USE_ETCD_TLS
Etcd CA Cert| string | Path to CA certificate to be used when connecting to Etcd server | - | ANKA_ETCD_CA_CERT
Etcd Client Cert| string | Path to Etcd Client certificate to be used when connecting to Etcd server | - | ANKA_ETCD_CERT
Etcd Client Key| string | Path to Etcd Client Key to be used when connecting to Etcd server | - | ANKA_ETCD_CERT_KEY
Skip Etcd TLS verification | bool | Don't use TLS verification for Etcd Authentication | false | ANKA_SKIP_ETCD_TLS_VERIFICATION
Enable Etcd user login | bool | Enable Etcd user login when connecting to Etcd server | false | ANKA_USE_ETCD_LOGIN
Etcd Username | string | Etcd username to be used to login to Etcd server | - | ANKA_ETCD_USERNAME
Etcd Password | string | Etcd password to be used to login to Etcd server | - | ANKA_ETCD_PASSWORD
