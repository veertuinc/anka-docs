---
---

You can bring you own `.ipsw` and `.app` files to use with `anka create`. This is an alternative to specifying the version from `--list`. It supports three different methods:

##### 1. The specific macOS version from `--list`.

{{< hint warning >}}
**ARM USERS:** At the moment Apple only provides a public endpoint to list the latest macOS version. `anka create --list` will therefore only show a single version. We are working to get them to publically list all `ipsw`. In the meantime, Intel Anka does show all archived macOS versions for `.app`.
{{< /hint >}}

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

  bash$ anka create 11.7.2 11.7.2
  75% [|||||||||||||||||||||||||||||||||||||||||||||               ] 16:15 ETA
  ```

##### 2. The location/path to the `ipsw` on the host.

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 12.5.1 /Applications/macos-12.5.1.app
  . . .
  ```

##### 3. The URL to download the `ipsw` from.

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 12.5.1 https://myCompanyIntranet/UniversalMac_13.1_22C65_Restore.ipsw
  . . .
  ```