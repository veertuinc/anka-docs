Anka VM Templates support the following macOS versions:

- `12.x` (macOS Montrey)
- `11.5.x` + `11.6.x` (macOS Big Sur)
- `10.15.x` (macOS Catalina)
- `10.14.x` (macOS Mojave)
- `10.13.x` (macOS High Sierra)
- `10.12.x` (macOS Sierra)

{{< hint warning >}}
Monterey installation inside of the VM seems to be much longer than previous versions. Please be patient and **DO NOT** interfere with the installation manually. If you want to confirm it's progressing you can launch the VM viewer with `anka view`.
{{< /hint >}}

{{< hint warning >}}
Monterey VMs require `ablk` as the disk driver. The `anka create` command will set this, but be sure to choose it in the `Create new VM` section of the Anka application instead of `virtio-blk`.
{{< /hint >}}