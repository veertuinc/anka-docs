---
date: 2019-07-03T22:24:47-05:00
title: " Configuring Certificate based authentication"
linkTitle: "Certificate based authentication"
weight: 8
description: >
  How to set up certificate based authentication.
---

> This requires Anka Enterprise (or higher) license.

## Obtain a Root CA certificate

For this tutorial you will need a CA certificate with it's private key. For more information about CAs, see https://en.wikipedia.org/wiki/Certificate_authority.

For the rest of this section, CA certificate and key will be referred to as **anka-ca-crt.pem** and **anka-ca-key.pem**. 

If you don’t have one, you can create it with openssl and add it to your keychain:

```shell
cd ~

export KEYCHAIN="$HOME/Library/Keychains/login.keychain-db"

openssl req -new -Nodes -x509 -days 365 -keyout anka-ca-key.pem -out anka-ca-crt.pem -subj "/C=$COUNTRY/ST=$STATE/L=$LOCATION/O=$ORGANIZATION/OU=$ORG_UNIT/CN=$CA_CN"

# Add the Root CA to the System keychain so the Root CA is trusted
sudo security add-trusted-cert -d -k /Library/Keychains/System.keychain anka-ca-crt.pem
```

## Configuring Server TLS

> The **Controller certificate** is not part of the authentication process and doesn't need to be derived from the CA you just generated. This means that you can use certificates supplied by your organization or a 3rd party.

If you do not have TLS certificates for your Controller, you can create them now:

```shell
export Controller_SERVER_IP="127.0.0.1"

openssl genrsa -out anka-controller-key.pem 4096

openssl req -new -nodes -sha256 -key anka-controller-key.pem -out anka-controller-csr.pem -subj "/C=$COUNTRY/ST=$STATE/L=$LOCATION/O=$ORGANIZATION/OU=$ORG_UNIT/CN=$CONTROLLER_CN" -reqexts SAN -extensions SAN -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nextendedKeyUsage = serverAuth\nsubjectAltName=IP:$CONTROLLER_SERVER_IP"))

openssl x509 -req -days 365 -sha256 -in anka-controller-csr.pem -CA anka-ca-crt.pem -CAkey anka-ca-key.pem -CAcreateserial -out anka-controller-crt.pem -extfile <(echo subjectAltName = IP:$Controller_SERVER_IP)
```

Ensure that the certificate has **Signature Algorithm: sha256WithRSAEncryption** using `openssl x509 -text -noout -in ~/anka-controller-crt.pem | grep Signature` (https://support.apple.com/en-us/HT210176)

### Installing for the Mac Controller & Registry

Edit `/usr/local/bin/anka-controllerd` in the following manner:

1. Change `LISTEN_ADDRESS=":80"` to `LISTEN_ADDRESS=":443"`

> SSL will work on any port you want.

2. Append the following parameters on the end of the **$CONTROLLER_BIN** line:

    ```shell
    --server-cert /Users/$USER_WHERE_CERTS_ARE/anka-controller-crt.pem --server-key /Users/$USER_WHERE_CERTS_ARE/anka-controller-key.pem --use-https
    ```

> The registry runs as root; **~/** = **/var/root**.

### Installing for the Linux/Docker Controller & Registry

You need to first mount a directory containing the certificates on the docker container. Create a directory and copy your certificates to that directory.

Then, within the `docker-compose.yml`:

1. Change the **anka-controller** ports from `80:80` to `443:80`.
2. Modify **ANKA_REGISTRY_ADDR** to use `https://`.
3. Uncomment the highlighted lines shown below and modify `****EDIT_ME****` to the location you created your certificates in:

{{< highlight dockerfile "hl_lines=11 13" >}}

. . .

  anka-controller:
    build:
       context: .
       dockerfile: anka-controller.docker
    ports:
       - "80:80"
    # To change the port, change the above line: - "CUSTOM_PORT:80"
    ######   EDIT HERE FOR TLS  ########
    # volumes:
      # Path to ssl certificates directory 
      # - ****EDIT_ME****:/mnt/cert
      
. . .

{{</ highlight >}}

Now let’s configure the Controller service to use those certificates. Search for the environment variables **USE_HTTPS**, **SERVER_CERT** and **SERVER_KEY** in `docker-compose.yml`, uncomment the lines, and then modify the `****EDIT_ME****` with the name of your certificate:

{{< highlight dockerfile "hl_lines=28 31 33" >}}

. . .

 anka-controller:
    build:
       context: .
       dockerfile: anka-controller.docker
    ports:
       - "443:80"
    # To change the port, change the above line: - "CUSTOM_PORT:80"
    ######   EDIT HERE FOR TLS  ########
    volumes:
      # Path to ssl certificates directory 
      - /home/ubuntu:/mnt/cert
    depends_on:
       - etcd
      #  - beanstalk
       - anka-registry
    restart: always
    environment:
      # Address of anka registry. this address will be passed to your build Nodes
      ANKA_REGISTRY_ADDR: https://127.0.0.1:8089

. . .

      ######   EDIT HERE FOR TLS ########

      # Use https, if enabled Controller will use tls for http communication
      # USE_HTTPS:              --use-https   

      # Server certificate pem
      # SERVER_CERT:            --server-cert /mnt/cert/****EDIT_ME**** 
      # Server private key pem
      # SERVER_KEY:             --server-key /mnt/cert/****EDIT_ME****
. . .

{{</ highlight >}}

### Test the Configuration

Start or restart your Controller and test the new TLS configuration using `https://`. You can also try using `curl -v https://$HOST/api/v1/status -k`.

> **Ensure that your Registry URL is set to use https:// as well!**

If that doesn’t work, try to repeat the above steps and validate that the file names and paths are correct. If you are still having trouble, debug the system as explained in the Debugging Controller section.

## Creating Node Certificates

The Controller's authentication module uses the Root CA (anka-ca-crt.pem) to authenticate the Node certificates. When the Node sends the requests to the Controller, it will present it's certificates. Those certificates will then be validated against the configured CA.

> The **Common Name (/CN=)** of the Node certificates will be used as the username and the **Organization (/O=)** fields will be used as groups (relevant for the Enterprise Plus License).

You can use the following openssl commands to create Node certificates (make sure you're in the directory of the anka-ca-crt/key.pem):

```shell
openssl genrsa -out node-$NODE_NAME-key.pem 4096

openssl req -new -sha256 -key node-$NODE_NAME-key.pem -out node-$NODE_NAME-csr.pem -subj "/C=$COUNTRY/ST=$STATE/L=$LOCATION/O=$ORGANIZATION/OU=$ORG_UNIT/CN=$NODE_NAME"

openssl x509 -req -days 365 -sha256 -in node-$NODE_NAME-csr.pem -CA anka-ca-crt.pem -CAkey anka-ca-key.pem -CAcreateserial -out node-$NODE_NAME-crt.pem
```

## Configuring the Controller with the CA Root certificate and enabling authentication

### Installing for the Mac Controller & Registry

Edit the `/usr/local/bin/anka-controllerd` and add the following onto the end of the **$CONTROLLER_BIN** line:

```shell
--ca-cert ~/anka-ca-crt.pem --enable-auth
```

### Installing for the Linux/Docker Controller & Registry

Within the `docker-compose.yml`, search for the environment variables **ENABLE_AUTH** and **CA_CERT**. Edit them so they will look like the configuration below (assuming that a certificate folder is already mounted at /mnt/cert).

{{< highlight dockerfile "hl_lines=26 27" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "443:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:  

. . .

     USE_HTTPS:              --use-https  
     # Server certificate pem
     SERVER_CERT:            --server-cert /mnt/cert/anka-controller-crt.pem
     # Server private key pem
     SERVER_KEY:             --server-key /mnt/cert/anka-controller-key.pem
     ENABLE_AUTH:            --enable-auth 
     CA_CERT:                --ca-cert /mnt/cert/anka-ca-crt.pem

. . .

{{</ highlight >}}

## Joining your Node to the Controller with the Node certificate

> If you previously joined your Nodes to the Controller, you'll want to `sudo ankacluster disjoin` on each before proceeding (if it hangs, use `ps aux | grep anka_agent | awk '{print $2}' | xargs kill -9` and try disjoin again).

Copy both the Node certificates (node-$NODE_NAME-crt.pem, node-$NODE_NAME-key.pem) and the anka-ca-crt.pem to the Node.

Then, use the `ankacluster` command to connect it to the Controller in the following manner:

```shell
sudo ankacluster join https://$HOST --cert node-$NODE_NAME-crt.pem --cert-key node-$NODE_NAME-key.pem --cacert anka-ca-crt.pem
You should see output similar to the following:
Testing connection to Controller...: OK
Testing connection to registry….: OK
Ok
Cluster join success
```

### Testing Controller API authentication

Restart your Controller & Registry.

Then, test the status endpoint with curl:

```shell
curl -v https://$HOST/api/v1/status -k
```

The response you should get is a **401 Authentication Required** similar to below:

```shell
> GET /api/v1/status HTTP/2
> Host: localhost:8090
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

If this is the response you get, it means the authentication module is working.

Let’s try to get a response using the client certificate we created.

Execute the same command, but now pass client certificate and key (assuming those files are called ‘node-$NODE_NAME-crt.pem’ and ‘node-$NODE_NAME-key.pem’).
```shell
curl -v https://$HOST/api/v1/status -k --cert node-$NODE_NAME-crt.pem --key node-$NODE_NAME-key.pem
```
If everthing is configured correctly, you should see something like this.

```shell
*   Trying 127.0.0.1...

. . .

> GET /api/v1/status HTTP/2
> Host: 127.0.0.1:8080
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
{"status":"OK","message":"","body":{"status":"Running","version":"1.7.0-4e6617d3","registry_address":"https://127.0.0.1:8081","registry_status":"Running","license":"enterprise plus"}}
* Connection #0 to host 127.0.0.1 left intact
* Closing connection 0
```

## Configuring root/superuser token for Controller authentication

Your Nodes are now able to authenticate with their certificates. However, you also want to setup a root/superuser for the Controller Dashboard.

> Your root token should be at least 10 characters long.

### Configuring for the Mac Controller & Registry

Edit `/usr/local/bin/anka-controllerd` and append `--root-token $ROOT_TOKEN` to the end of the **$CONTROLLER_BIN** line.

### Configuring for the Linux/Docker Controller & Registry

Edit `docker-compose.yml` and modify the environment variable **ROOT_TOKEN**:

{{< highlight dockerfile "hl_lines=25" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "443:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:     
     USE_HTTPS:              --use-https  
     # Server certificate pem
     SERVER_CERT:            --server-cert /mnt/cert/anka-controller-crt.pem
     # Server private key pem
     SERVER_KEY:             --server-key /mnt/cert/anka-controller-key.pem
     ENABLE_AUTH:            --enable-auth 
     CA_CERT:                --ca-cert /mnt/cert/anka-ca-crt.pem
     ROOT_TOKEN:             --root-token 0987654321

. . .

{{</ highlight >}}

### Testing root Controller Dashboard authentication
If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear. Enter the token you specified and ensure that it logs you in.