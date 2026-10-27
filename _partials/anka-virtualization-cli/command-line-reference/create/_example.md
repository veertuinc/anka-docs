

You can bring your own `.ipsw` and `.app` files to use with `anka create`. This is an alternative to specifying the version from `--list`. It supports three different methods:

##### 1. The specific macOS version from `--list`

{{< hint info >}}
**ARM (Apple Silicon):** `anka create --list` shows available `.ipsw` restore versions. Specify one by version number (for example `27.2`) or use `latest`.

**Intel:** `anka create --list` shows archived macOS versions for `.app` installers as well as restore images.
{{< /hint >}}

  ```bash
  bash$ anka create --list
  +---------------+---------+------------+
  | version       | build   | post_date  |
  +---------------+---------+------------+
  | 27.2 (latest) | 26B123  | 2026-10-26 |
  +---------------+---------+------------+
  | 27.0.1        | 26A434  | 2026-09-28 |
  +---------------+---------+------------+
  | 27.0          | 26A428  | 2026-09-14 |
  +---------------+---------+------------+
  | 26.6.2        | 25G83   | 2026-08-17 |
  . . .

  bash$ anka create my-ci-vm 27.2
  75% [|||||||||||||||||||||||||||||||||||||||||||||               ] 16:15 ETA
  ```

##### 2. The location/path to the `ipsw` or `.app` on the host

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 27.2 ~/Downloads/UniversalMac_27.2_26B123_Restore.ipsw
  . . .
  ```

  Intel only (`.app` installer):

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 15.7.4 /Applications/macos-15.7.4.app
  . . .
  ```

##### 3. The URL to download the `ipsw` from (.app not supported)

  ```bash
  bash$ anka create --cpu-count 5 --disk-size 100G 27.2 https://myCompanyIntranet/UniversalMac_27.2_26B123_Restore.ipsw
  . . .
  ```
