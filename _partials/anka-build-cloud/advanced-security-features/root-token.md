

Enabling root token authentication is a simple process. The root user has full permissions to the Controller UI and APIs for both the Controller and Registry. It is however not used for Node communication.

{{< hint warning >}}
###### Important
- Enabling the Root Token is required in all of our Auth features to function.
- The root token must be at least 10 characters long.
- The root token set for both the Registry and Controller must match.
- Keep this token safe. We don't recommend trying to use the root token for API calls in scripts due to security risk.
- Enabling RTA will block any access for Anka Nodes joined to the primary interface/port for the controller. You will need to set up [one of the other Authentication methods]({{< relref "anka-build-cloud/Advanced Security Features/_index.md" >}}) supported by the `ankacluster join` command. You can [expose a queue only interface]({{< relref "anka-build-cloud/configuration-reference.md#separate-queue-interface" >}}) instead which can be used to join your nodes **ONLY** if you cannot use credentials.
{{< /hint >}}

---

### How to configure RTA

#### Linux/Docker Package

{{< hint info >}}With our docker package, each service is split up into its own container. You can enable a root token for either the controller, registry, or both.{{< /hint >}}

Edit the `docker-compose.yml` and add both `ANKA_ENABLE_AUTH` and `ANKA_ROOT_TOKEN` environment variables:

{{< highlight dockerfile "hl_lines=16 17" >}}

. . .

anka-controller:
   build:
      context: .
      dockerfile: anka-controller.docker
   ports:
      - "80:80"
   volumes:
     # Path to ssl certificates directory
     - /home/ubuntu:/mnt/cert
   depends_on:
      - etcd
   restart: always
   environment:
     ANKA_ENABLE_AUTH: "true"
     ANKA_ROOT_TOKEN: "1111111111"
     # ANKA_ENABLE_API_KEYS="true"

anka-registry:
   build:
      context: .
      dockerfile: anka-registry.docker
   ports:
      - "8089:8089"
   . . .
   environment:
     ANKA_ENABLE_AUTH: "true"
     ANKA_ROOT_TOKEN: "1111111111"
     # ANKA_ENABLE_API_KEYS="true"
. . .

{{</ highlight >}}

<!-- #### MacOS Package

Edit `/usr/local/bin/anka-controllerd` and add:

```bash
export ANKA_ENABLE_AUTH="true"
export ANKA_ROOT_TOKEN="{min10chartoken}"
```

{{< hint info >}}For the macOS package, this enabled the Root Token for both Controller and Registry. There is no way to enable the ENV for one and not the other.{{< /hint >}} -->


### Testing RTA

If everything is configured correctly, you can visit your Controller Dashboard and a login box should appear.

![root token login]({{< siteurl >}}images/anka-build-cloud/advanced-security-features/controller-root-token-login.png)

Enter the token you specified and ensure that it logs you in.

Finally, you can test the API using:

```bash
❯ curl -H "Authorization: Basic $(echo -ne "root:1111111111" | base64)" http://anka.registry:8089/registry/status
{"status":"OK","body":{"status":"Running","version":"1.19.0-309d8150"},"message":""}
```