```shell

❯ anka show 12.0-beta | grep status
| status  | stopped Oct 19 07:34:43 2021         |

❯ anka run 12.0-beta hostname
12-0-beta.local

❯ anka show 12.0-beta | grep status
| status  | running since Oct 19 07:34:59 2021   |

❯ TEST=123 anka run -e TEST 12.0-beta bash -c "env | grep TEST && echo \${TEST}"
TEST=123
123

```