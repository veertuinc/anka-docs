

You can bring your own `.ipsw` and `.app` files to use with `anka create`. This is an alternative to specifying the version from `--list`. It supports three different methods:

##### 1. The specific macOS version from `--list`

{{< hint info >}}
**ARM (Apple Silicon):** `anka create --list` shows available `.ipsw` restore versions. Specify one by version number (for example `26.6.2`) or use `latest`.

**Intel:** `anka create --list` shows archived macOS versions for `.app` installers as well as restore images.
{{< /hint >}}

  ```bash
  bash$ anka create --list
  +-----------------+---------+------------+
  | version         | build   | post_date  |
  +-----------------+---------+------------+
  | 26.6.2 (latest) | 25G83   | 2026-08-17 |
  +-----------------+---------+------------+
  | 26.6.1          | 25G76   | 2026-08-06 |
  +-----------------+---------+------------+
  | 26.6            | 25G72   | 2026-07-27 |
  +-----------------+---------+------------+
  | 26.5.2          | 25F84   | 2026-06-29 |
  . . .

  bash$ anka create my-ci-vm 26.6.2
  75% [|||||||||||||||||||||||||||||||||||||||||||||               ] 16:15 ETA
  ```

##### 2. The location/path to the `ipsw` or `.app` on the host

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 26.6.2 ~/Downloads/UniversalMac_26.6.2_25G83_Restore.ipsw
  . . .
  ```

  Intel only (`.app` installer):

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 15.7.4 /Applications/macos-15.7.4.app
  . . .
  ```

##### 3. The URL to download the `ipsw` from (.app not supported)

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 26.6.2 https://myCompanyIntranet/UniversalMac_26.6.2_25G83_Restore.ipsw
  . . .
  ```
