

Anka VM Templates support the following macOS versions:

- `13.x` (macOS Ventura)
- `12.x` (macOS Monterey)
- `11.6.x` (macOS Big Sur)
- `10.15.x` (macOS Catalina)
- `10.14.x` (macOS Mojave)

{{< hint warning >}}
Monterey installation inside of the VM seems to be much longer than previous versions. Please be patient and **DO NOT** interfere with the installation manually. If you want to confirm it's progressing you can launch the VM viewer with `anka view`.
{{< /hint >}}

{{< hint warning >}}
Starting in Monterey, VMs require `ablk` as the disk driver.
{{< /hint >}}