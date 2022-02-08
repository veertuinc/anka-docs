```shell

❯ anka show 12.2.0-arm | grep status
| status  | stopped Oct 19 07:34:43 2021         |

❯ anka run 12.2.0-arm hostname
12-2-0-arm.local

❯ anka show 12.2.0-arm | grep status
| status  | running since Oct 19 07:34:59 2021   |

❯ anka run 12.2.0-arm bash -c "env"           
SHELL=/bin/zsh
TMPDIR=/var/folders/3f/1zss6fl15db6k9bhvcrw4wcr0000gn/T/
USER=anka
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.xZYuuCeCwR/Listeners
__CF_USER_TEXT_ENCODING=0x1F5:0x0:0x0
PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
PWD=/Users/anka
XPC_FLAGS=0x0
XPC_SERVICE_NAME=com.veertu.anka.addons.ankarun
SHLVL=1
HOME=/Users/anka
LOGNAME=root
_=/usr/bin/env

❯ TEST=123 anka run -e TEST 12.2.0-arm bash -c "env | grep TEST && echo \${TEST}"
TEST=123
123

```