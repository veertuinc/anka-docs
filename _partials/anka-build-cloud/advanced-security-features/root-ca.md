---
---

If you don’t have a Root CA yet, you can create it with openssl:

```shell
cd ~
openssl req -new -nodes -x509 -days 365 -keyout anka-ca-key.pem -out anka-ca-crt.pem \
  -subj "/O=MyGroup/OU=MyOrgUnit/CN=MyUser"
```

{{< hint info >}}
You can add the Root CA to the System keychain so the Root CA is trusted and you can avoid warnings when you go to access the Controller UI.
```bash
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain anka-ca-crt.pem
```
{{< /hint >}}