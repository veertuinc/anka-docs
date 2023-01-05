---
---

While you can bring your own `.app` or `.ipsw` files when creating VMs, we allow you to specify `anka create -a latest` to target the latest macOS or `anka create -a VERSION` where `VERSION` is the specific macOS version. To get a list of macOS versions available, see `anka create --list`:

```bash
bash$ anka create -l
+---------------+---------+----------------------+
| version       | build   | post_date            |
+---------------+---------+----------------------+
| 11.7.2        | 20G1020 | Dec 13 13:14:48 2022 |
+---------------+---------+----------------------+
| 12.6.2        | 21G320  | Dec 13 13:13:39 2022 |
+---------------+---------+----------------------+
| 13.1 (latest) | 22C65   | Dec 13 13:08:36 2022 |
+---------------+---------+----------------------+
| 13.0.1        | 22A400  | Nov 9 13:02:34 2022  |
. . .

bash$ anka create -a 11.7.2 11.7.2
 75% [|||||||||||||||||||||||||||||||||||||||||||||               ] 16:15 ETA
```