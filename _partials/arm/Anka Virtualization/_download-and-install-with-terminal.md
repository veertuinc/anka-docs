![terminal installation]({{< siteurl >}}images/anka-virtualization/terminal-installation.png)

```shell
FULL_FILE_NAME="$(curl -Ls -r 0-1 -o /dev/null -w %{url_effective} https://veertu.com/downloads/anka-virtualization-arm | cut -d/ -f5)"
curl -S -L -o ./$FULL_FILE_NAME https://veertu.com/downloads/anka-virtualization-arm
sudo installer -pkg $FULL_FILE_NAME -tgt /
``` 