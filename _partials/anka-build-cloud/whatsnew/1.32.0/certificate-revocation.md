

Starting in 1.32.0, users can now revoke certificates without needing to change the CA or permissions.

#### Generating the Certificate Revocation List

1. Locally on your machine you'll create `openssl.cnf` (anywhere on your machine), containing various definitions:
  ```bash
  [ ca ]
  default_ca = CA_default                 # The name of the CA configuration to be used.
                                          # can be anything that makes sense to you.
  [ CA_default ]
  dir = .                                 # Directory where everything is kept
  certs = $dir                            # Directory where the issued certs are kept
  crl_dir = $dir                          # Directory where the issued crl are kept
  database = $dir/index.txt               # database index file.
  #unique_subject = no                    # Set to 'no' to allow creation of
                                          # several certificates with same subject.
  new_certs_dir = $dir                    # Default directory for new certs.

  certificate = $dir/anka-ca-crt.pem           # The CA certificate
  serial = $dir/serial                    # The current serial number
  crlnumber = $dir/crlnumber              # The current crl number
                                          # must be commented out to leave a V1 CRL
  crl = $dir/crl.pem                      # The current CRL
  private_key = $dir/anka-ca-key.pem           # The private key
  RANDFILE    = $dir/.rand                # private random number file

  x509_extensions = usr_cert              # The extentions to add to the cert

  name_opt = ca_default                   # Subject Name options
  cert_opt = ca_default                   # Certificate field options

  default_days    = 365                   # how long to certify for
  default_crl_days= 30                    # how long before next CRL
  default_md    = sha1                    # use public key default MD
  preserve    = no                        # keep passed DN ordering

  policy = policy_match
  ```

2. Next, create `index.txt` and `crlnumber` and store them in a centralized location:

{{< hint warning >}}
Any revocations must happen against the latest `index.txt` and `crlnumber` (`openssl.cnf` is optional). It's best to store these in a repo or somewhere that admins revoking can pull the latest versions and then commit them once complete.
{{< /hint >}}

  ```bash
  touch index.txt         # stores revoked certificates database
  echo "01" > crlnumber   # needs to be only initialized once
  ```

3. Generate your first Certificate Revocation List (a.k.a "CRL"):

  ```bash
  openssl ca -gencrl \
    -config openssl.cnf \
    -keyfile anka-ca-key.pem \
    -cert anka-ca-crt.pem \
    -out crl.pem \
    -crldays 365
  ```

5. If Certificate Authentication is enabled for both the Controller and Registry, you'll need to set `ANKA_CRL` in both configs to the Revocation List File (.pem). If using Docker, you'll need to attach a mount/volume that contains this file and target the destination/location inside of the container.
6. Generate your client certificates with `openssl` as you normally would, **using the same root CA you used to generate the CRL**.


#### Revoking a certificate

1. When you need to revoke a certificate, you can use the following to add the cert to the `index.txt` database file:

  ```bash
  openssl ca -config openssl.cnf \
    -revoke node-Veertu.local-crt.pem \
    -keyfile anka-ca-key.pem \
    -cert anka-ca-crt.pem
  ```

2. Once revoked, you must re-generate the CRL file and update it on the Controller and Registry, **restarting them once updated**:

  ```bash
  openssl ca -gencrl \
    -config openssl.cnf \
    -keyfile anka-ca-key.pem \
    -cert anka-ca-crt.pem \
    -out crl.pem \
    -crldays 365
  ```

##### Some things to note

- You can check if a certificate is revoked with `openssl verify -crl_check -CAfile anka-ca-crt.pem -CRLfile crl.pem node-Veertu.local-crt.pem`.
- The `index.txt`, `crlnumber`, and certs generated should be stored in a centralized location. We recommend a repo or a server with openssl on it that only admins have access to. Any changes made should be checked into the repo so that others can pull them and start from the latest version of the revocation DB (index.txt).
- If the `crl.pem` expires, the controller and registry will fail.
- The `crl.pem` must be generated with CA Root cert that the Controller and Registry are using.
