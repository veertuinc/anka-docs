```shell
> anka create --help
usage: create [options] name

   Creates a VM

options:
  -m,--ram-size <val>
                     Set the RAM size
  -c,--cpu-count <val>
                     Set the number of CPU cores
  -d,--disk-size <val>
                     Set the disk size
  -a,--app <val>     Path to the macOS restore image
```

{{< hint warning >}}
`--app` is not like the 2.X Anka intel version. It requires a restore image from apple, not a .app installer. For now, we will automatically download the latest Monterey restore image from apple on `anka create`.
{{< /hint >}}