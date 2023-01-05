```shell

❯ anka show 12.6 | grep status
| status  | suspended                            |

❯ anka run 12.6 hostname
Ankas-Virtual-Machine.local

❯ anka run 12.6 zsh -lc "env"
XPC_SERVICE_NAME=com.veertu.anka.addons.ankarun
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.uf92KIf4B0/Listeners
PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
XPC_FLAGS=0x0
LOGNAME=anka
USER=anka
HOME=/Users/anka
SHELL=/bin/zsh
TMPDIR=/var/folders/vh/lf52c8657b902x4p2n3splkm0000gn/T/
__CF_USER_TEXT_ENCODING=0x1F5:0x0:0x0
SHLVL=0
OLDPWD=/Users/anka
PWD=/Users/anka
_=/usr/bin/env

❯ TEST=123 anka run -e TEST 12.6 bash -c "env | grep TEST && echo \${TEST}"
TEST=123
123

```
