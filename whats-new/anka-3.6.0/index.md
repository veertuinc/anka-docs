---
date: 2024-12-10T01:00:00-00:00
title: "Anka Virtualization 3.6.0"
---

## Ability to install multiple versions of Anka on the same host

**You cannot use this feature for version less than 3.6.0.**

Starting in 3.6.0, you can install multiple versions of Anka (>= 3.6.0) on the same host. To do this, install one version, then rename the `/Applications/Anka.app` to something like `Anka-3.6.0.app`. Then, install the next version.

You'll then have two versions of Anka installed on your host under `/Applications`: `Anka.app` and `Anka-3.6.0.app`. You can then use `sudo anka-select` to switch between them.

```bash
❯ sudo anka-select --list
  3.6.0.196.2652630	/Applications/Anka-3.6.0-rc1.app
* 3.6.0.197	/Applications/Anka.app

❯ sudo anka-select --switch /Applications/Anka-3.6.0-rc1.app
❯ sudo anka-select -p
/Applications/Anka-3.6.0-rc1.app
❯ anka version
Anka Build Enterpriseplus version 3.6.0 (build 196.2652630)


❯ sudo anka-select --switch /Applications/Anka.app
❯ sudo anka-select -p
/Applications/Anka.app
❯ anka version
Anka Build Enterpriseplus version 3.6.0 (build 197)
```

