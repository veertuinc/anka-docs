### Download

You can find our releases on the [Anka Gitlab Runner GitHub Repo](https://github.com/veertuinc/gitlab-runner/releases).

You can then curl down the version you need for your OS and architecture:

```shell
LOCATION="/tmp/anka-gitlab-runner"
mkdir -p $LOCATION
cd $LOCATION
curl -L -o anka-gitlab-runner.tar.gz https://github.com/veertuinc/gitlab-runner/releases/download/1.0/gitlab-runner_1.0_darwin_amd64.tar.gz
tar -xzvf anka-gitlab-runner.tar.gz
BIN_LOCATION="/usr/local/bin/anka-gitlab-runner"
cp -rfp $LOCATION/gitlab-runner-darwin-* $BIN_LOCATION
chmod +x $BIN_LOCATION
```

Confirm that the binary now exists:

```shell
❯ anka-gitlab-runner status                
Runtime platform                                    arch=amd64 os=darwin pid=19465 revision=1fb7c012 version=12.10.0~beta.1321.g1fb7c012
anka-gitlab-runner: Service is not running.
```

### Install

```shell
❯ anka-gitlab-runner install  
Runtime platform arch=amd64 os=darwin pid=39598 revision=c505cd17 version=12.10.0~beta.1318.gc505cd17
```

The `install` command will place an `anka-gitlab-runner.plist` file into `~/Library/LaunchAgents/` (`/Library/LaunchAgents` if root user) so that the runner is available on restarts.

The `.plist` will contain `--working-directory` (which is set to the current location when running `anka-gitlab-runner install`) and `--config` (set to `"${current location}/.gitlab-runner/anka-config.toml`).

> You can uninstall with `anka-gitlab-runner uninstall`