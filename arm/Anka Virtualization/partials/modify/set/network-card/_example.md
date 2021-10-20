> WiFi interfaces are not supported yet

```shell
❯ anka describe 11.0.1
. . .
network_cards

+------------+--------+--------+-----------+
|   pci_slot | type   | mode   |    pci_id |
+============+========+========+===========+
|         28 | anet   | shared | 538975646 |
+------------+--------+--------+-----------+
. . .

❯ anka modify 11.0.1 set network-card 0 -t bridge

❯ anka describe 11.0.1
. . .
network_cards

+------------+--------+--------+-----------+
|   pci_slot | type   | mode   |    pci_id |
+============+========+========+===========+
|         28 | anet   | bridge | 538975646 |
+------------+--------+--------+-----------+
. . .
```
