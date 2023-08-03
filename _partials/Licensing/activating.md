---
---

{{< hint info >}}
**Licensing is not necessary if you're using Anka Develop.**
{{< /hint >}}

{{< hint info >}}
You can see how many hosts or cores a license has available by running: anka license show -k {LICENSE}
{{< /hint >}}

{{< hint info >}}
Corporate networks can sometimes block access to our licensing server. The machine you're activing the license on will need to reach the internet, or, support can provide the licensing server URL upon request.
{{< /hint >}}

{{< hint info >}}
If you're using a proxy, Anka recognizes the standard `http_proxy` and `https_proxy` env variables. However, `sudo` will not pass the environment by default, so activation should be called with `sudo -E anka license activate {LICENSE}` to pass the current user's `*_proxy` envs into sudo.
{{< /hint >}}

```shell
❯ anka license     
Usage: anka license [OPTIONS] COMMAND [ARGS]...

  Control Licensing

Options:
  --help  Display usage information

Commands:
  accept-eula  Accept EULA (requires root privileges)
  activate     Activate license key (requires root...
  remove       Remove the current license (requires root...
  show         Show license information
  validate     Validate the current license
  
❯ sudo anka license activate {LICENSE}
License activated

❯ anka license show
+--------------+-------------------------+
| license_type | com.veertu.anka.entplus |
+--------------+-------------------------+
| status       | valid                   |
+--------------+-------------------------+
| expires      | 30-mar-2021             |
+--------------+-------------------------+
```