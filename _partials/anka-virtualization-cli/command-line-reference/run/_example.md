#### Shell Configuration Files / Environment

The [`anka run`]({{< relref "anka-virtualization-cli/command-line-reference.md#run" >}}) command uses the "default shell" that Apple's API provides inside of macOS. It will NOT source any configuration by default. However, you can use `bash` and `zsh` command with `anka run` to source them:

```shell
anka run 12.6 bash -c "echo 'export TEST_ZSHRC=yes' >> ~/.zshrc"
anka run 12.6 bash -c "echo 'export TEST_ZPROFILE=yes' >> ~/.zprofile"
anka run 12.6 bash -c "echo 'export TEST_PROFILE=yes' >> ~/.profile"
anka run 12.6 bash -c "echo 'export TEST_BASH_PROFILE=yes' >> ~/.bash_profile"

anka run 12.6 env
XPC_SERVICE_NAME=com.veertu.anka.addons.ankarun
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.zX8qHCS8ON/Listeners
PATH=/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin
XPC_FLAGS=0x0
LOGNAME=anka
USER=anka
HOME=/Users/anka
SHELL=/bin/bash
TMPDIR=/var/folders/6c/8_zmsrb509j6xv075_9h22rm0000gn/T/
__CF_USER_TEXT_ENCODING=0x1F5:0x0:0x0
SHLVL=1

anka run 12.6 bash -c "env | grep TEST_"

anka run 12.6 bash -ic "env | grep TEST_"

anka run 12.6 bash -lc "env | grep TEST_"
TEST_BASH_PROFILE=yes

anka run 12.6 zsh -c "env | grep TEST_"

anka run 12.6 zsh -ic "env | grep TEST_"
TEST_ZSHRC=yes

anka run 12.6 zsh -lc "env | grep TEST_"
TEST_ZPROFILE=yes
```

{{< hint info >}}
To inherit the host's environment, use the `anka run -E` (existing VM variables will not be overridden by host's variables) or `-e MYENV` options. You can also pass them inside of a file like `anka run --env-file environment.txt`, where environment.txt is a text file in the form `VARIABLE=VALUE`, one variable per line.
{{< /hint >}}
