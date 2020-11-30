> If you leave out `--ram-size` and `--cpu-count`, Anka will choose for you based on your total CPU and RAM. It will try to halve the total CPU and RAM values, but has a min of 2CPU/4GB and a max of 8CPU/8GB

> We recommend naming your initial VM after the version of macOS

> **Be aware of the user you're executing Anka CLI commands as.** If you create VMs as root, they won't be available (when running `anka list`) to other users on the system and vice versa

> By default `anka create` creates a VM Template with the username `anka` and password `admin`. Environment variable are available to change these: `ANKA_DEFAULT_USER` and `ANKA_DEFAULT_PASSWD` (be sure to use `sudo -E` when issuing the create command)

> The VM creation should take around 30 minutes

> **Anka Develop licenses** default your VM in a stopped state (the only available state)

> **Anka Build licenses** default your VM in a suspended state

> **Catalina and lower** VMs are created with SIP/Kext Consent disabled by default. It's strongly advised to keep these settings for optimal Anka performance. If you need to re-enable SIP/Kext Consent, then use this command `anka modify {vmNameOrUUID} set custom-variable sys.csr-active-config 0`
