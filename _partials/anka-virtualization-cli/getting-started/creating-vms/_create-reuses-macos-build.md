The first `anka create` for a macOS build still downloads the IPSW and installs it. After the install, Anka stores a tag named `.base_<ProductBuild>` on that VM and deletes the downloaded IPSW from the image library.

The next `anka create` for the same ProductBuild clones that tag. It does not download the IPSW again. The clone copies the tag onto the new VM, so later creates can still reuse the disk data after you delete the first VM.

```bash
❯ anka create vm1 latest
❯ anka create vm2 latest
```

`vm1` installs from the IPSW. `vm2` clones the stored tag.

`latest` and an explicit version or IPSW share a tag only when they resolve to the same ProductBuild.


{{< hint info >}}
Set `ANKA_CREATE_FORCE=1` to skip the stored tag and install from the IPSW again:

```bash
❯ ANKA_CREATE_FORCE=1 anka create vm3 latest
```
{{< /hint >}}
