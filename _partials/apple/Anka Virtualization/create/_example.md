```shell
❯ anka create -a latest 12.2.0-arm
Installing UniversalMac_12.1_21C52_Restore.ipsw...
######################################################################## 100.0%
00c44c30-174a-4266-8833-89d6975754bd
```
{{< hint info >}}
The ipsw will be downloaded into `img_lib_dir`. You can find the location of this directory with `anka config img_lib_dir`. Not that these files can be deleted with `anka delete --cache`.
{{< /hint >}}
