---
date: 2019-07-03T22:24:47-05:00
title: " Configuring Certificate based authentication"
linkTitle: "Certificate based authentication"
weight: 8
description: >
  How to set up certificate based authentication.
---


## Getting or Creating a CA certificate

For this tutorial you will need a CA certificate with it’s private key. For more information about CAs - https://en.wikipedia.org/wiki/Certificate_authority.

For the rest of this section CA certificate and key will be referred to as ‘ca-crt.pem’ and ‘ca-key.pem’. If you don’t have one you can create one with openssl.

```
openssl req -new -nodes -x509 -days 365 -keyout ca-key.pem -out ca-crt.pem -subj "/CN=myOrganization"

```
This command will create two files: ca-crt.pem and ca-key.pem.

## Configuring Server TLS

The first step will be to configure tls for the server. The server certificates are not part of the authentication process and doesn’t need to be derived from the CA. This means that you can use certificates supplied by a 3rd party like “let’s encrypt” or use the certificates your organization supplies.

If you do not have tls certificates for your server, you can create them now. The following commands will create server certificates for a machine with a certain ip. Replace $MACHINE_IP with your server’s ip.

```
openssl genrsa -out controller-key.pem 4096
openssl req -new -nodes -sha256 -key controller-key.pem -out controller-csr.pem -subj "/CN=*" -reqexts SAN -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nsubjectAltName=IP:$MACHINE_IP"))
openssl x509 -req -days 365 -in controller-csr.pem -CA ca-crt.pem -CAkey ca-key.pem -CAcreateserial -out controller-crt.pem -extfile <(echo subjectAltName = IP:$MACHINE_IP)
```

### For Mac Controller Setup

Edit /usr/local/bin/anka-controllerd in the following manner.

Change 
```
LISTEN_ADDRESS=":80"
```
To
```
LISTEN_ADDRESS=":443"
```
Search for the row that looks like this
```
"$CONTROLLER_BIN" --listen_addr "$LISTEN_ADDRESS" --log_dir "$LOG_DIR" --data-dir "$DATA_DIR" $ANKA_REGISTRY $STANDALONE $ALLOW_EMPTY_REGISTRY
```
This is the controller’s run command. Append the following parameters to this line.

```
--server-cert $PATH_TO_SERVER_CERT --server-key $PATH_TO_SERVER_KEY --use-https
```
### For Docker Controller Setup

You need to mount a directory containing the certificates on the docker container. Create a directory and copy your certificates to that directory.

Edit `docker-compose.yml`. In the controller’s section add a volume pointing to the directory you just created mounted on `“/mnt/cert”`. (we will use /home/ubuntu/certs as an example).

Also change the ports forwarding to 443 (https).
```
anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "443:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu/cert:/mnt/cert
     
```

Now let’s add the path to our certificate and key. Search for the environment variables “USE_HTTPS”, “SERVER_CERT” and “SERVER_KEY” in `docker-compose.yml`. Edit them so they will look like the configuration below. If they do not exist - add them. Note that if you named your files differently the values for these parameters should match them (if you named your certificate my-crt.crt the SERVER_CERT value should look like ‘/mnt/cert/my-crt.crt’).

```
anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "443:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu/certs:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:     
     USE_HTTPS:              --use-https  
     # Server certificate pem
     SERVER_CERT:            --server-cert /mnt/cert/controller-crt.pem
     # Server private key pem
     SERVER_KEY:             --server-key /mnt/cert/controller-key.pem
     
```

## Test the configuration

Start or restart your controller and test the new TLS configuration.

Point your browser to the hostname or ip serving the controller dashboard using ‘https://’. You can also try using `curl -v https://$HOST/api/v1/status -k`.

If that doesn’t work, try to repeat the steps and validate that the file names and paths are correct. If you are still having trouble, debug the system as explained in the Debugging controller section.

#### Using client certificates

The controller’s authentication module uses the CA file passed to configuration to validate the client certificates. The file should contain one (or more) certificate authority that was used to create the client certificates.

When the client will send requests to the controller, it will present it’s certificates. Those certificates will then be validated against the configured CA.

The common name of the client certificates will be used as the user name and the organization fields will be used as groups (this is more relevant for authorization in enterprise plus version).

### Creating client certificates

You can use the following openssl commands to create client certificates.

```
openssl genrsa -out node-key.pem 4096
openssl req -new -sha256 -key node-key.pem -out node-csr.pem -subj "/CN=my_node/O=nodes"
openssl x509 -req -days 365 -in node-csr.pem -CA ca-crt.pem -CAkey ca-key.pem -CAcreateserial -out node-crt.pem

```

This assumes that your CA certificate and key are in the same directory and called ‘ca-crt.pem’ and ‘ca-key.pem’ respectively. If it’s not, use the correct path.

## Configuring the controller with the CA file and enabling authentication

### For Mac Controller Setup

Edit `/usr/local/bin/anka-controllerd`.

The last line should be the controller’s run command and should look similar to this (it might have your server certificate from before).
```
"$CONTROLLER_BIN" --listen_addr "$LISTEN_ADDRESS" --log_dir "$LOG_DIR" --data-dir "$DATA_DIR" $ANKA_REGISTRY $STANDALONE $ALLOW_EMPTY_REGISTRY
```
Append the following parameters to this line.
```
--ca-cert $PATH_TO_CA_CERT --enable-auth
```

### For Docker Controller Setup
Use the mounted certificate folder that was configured in the server certificate stage. Copy the CA certificate to that folder on the host (just the certificate without the key).

Edit docker-compose.yml.

Search for the environment variables “CA_CERT” and “ENABLE_AUTH”. Edit them so they will look like the configuration below. (assuming that a certificate folder is already mounted at /mnt/cert).
```
anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "80:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu/certs:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:     
     USE_HTTPS:              --use-https  
     # Server certificate pem
     SERVER_CERT:            --server-cert /mnt/cert/controller-crt.pem
     # Server private key pem
     SERVER_KEY:             --server-key /mnt/cert/controller-key.pem
     ENABLE_AUTH:            --enable-auth 
     CA_CERT:                --ca-cert /mnt/cert/ca-crt.pem
```

### Testing your configuration for authentication
Restart your server. Your dashboard should not be working since authentication is now required.
We can test the status endpoint with curl.

Execute the following command.

```
`curl -v “https://$HOST/api/v1/status” -k`
```

The response you should get is a ‘401 Authentication required’ response like the response below.
```
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

Execute the same command, but now pass client certificate and key (assuming those files are called ‘node-crt.pem’ and ‘node-key.pem’).
```
`curl -v “https://$HOST/api/v1/status” -k --cert node-crt.pem --key node-key.pem` 
```
If everthing is configured correctly, you should see something like this.

```
> GET /api/v1/status HTTP/2
> Host: localhost:8090
> User-Agent: curl/7.58.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 200 
< cache-control: no-store
< content-type: application/json
< content-length: 188
< date: Thu, 28 Nov 2019 17:02:02 GMT
< 
{"status":"OK","message":"","body":{"status":"Running","version":"1.3.1-13123799","registry_address":"https://10.0.0.3:8089","registry_status":"Running","license":"enterprise"}}

```

## Joining Anka Client to Controller with client certificates

If you have any Anka nodes connected to your controller they now need the client certificate in order to work.

Copy the client certificate and key to the mac you are trying to join. Also copy the CA certificate used to create the controller certificates in case they are self signed (only copy the certificate, not the key).

Use `ankacluster` command to connect the agent to the controller in the following manner.

```
sudo ankacluster join https://$HOST --cert node-crt.pem --cert-key node-key.pem --cacert ca-crt.pem
You should see output similar to the following:
Testing connection to controller...: OK
Testing connection to registry….: OK
```

## Configuring root token for the Controller Portal

Your nodes are working with client certificates and your api can be used with client certificates also. Now, the controller portal can be secured with the root token authentication option.

Your root token should be at least 10 notes long. For this example we will use 0987654321 but you can generate a strong token for securing your Controller Portal.

### For Mac Controller

Edit `/usr/local/bin/anka-controllerd`.

The last line should be the controller’s run command and should look similar to this (it might have your certificates from before).
```
"$CONTROLLER_BIN" --listen_addr "$LISTEN_ADDRESS" --log_dir "$LOG_DIR" --data-dir "$DATA_DIR" $ANKA_REGISTRY $STANDALONE $ALLOW_EMPTY_REGISTRY

```
Append `--root-token 0987654321` to this line.

### For Docker Controller

Edit `docker-compose.yml`.

Search for the environment variable “ROOT_TOKEN”. Edit it so it will look like the configuration below.
```
anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "80:80"
   # To change the port, change the above line: - "CUSTOM_PORT:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu/certs:/mnt/cert
   depends_on:
      - etcd
     #  - beanstalk
   restart: always
   environment:     
     USE_HTTPS:              --use-https  
     # Server certificate pem
     SERVER_CERT:            --server-cert /mnt/cert/controller-crt.pem
     # Server private key pem
     SERVER_KEY:             --server-key /mnt/cert/controller-key.pem
     ENABLE_AUTH:            --enable-auth 
     CA_CERT:                --ca-cert /mnt/cert/ca-crt.pem
     ROOT_TOKEN:             --root-token 0987654321

```

## Testing your configuration for root token based authentication to the Controller Portal
Use the browser and go to your controller portal.

If everything is configured correctly, a login box should appear. Use the token string you created to login.


