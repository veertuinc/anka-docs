### Downloading

You can find our releases on the [Anka Gitlab Runner GitHub repo](https://github.com/veertuinc/gitlab-runner/releases).

You can then curl down the version you need for your OS and architecture:

> We'll be using an amd64 mac for our guide. However, the commands also work for Linux with slight alterations.

```shell
curl -L https://github.com/veertuinc/gitlab-runner/releases/download/1.0-rc1/anka-gitlab-runner-darwin-amd64.tar.gz
tar -xzvf anka-gitlab-runner-darwin-amd64.tar.gz
cp -rfp anka-gitlab-runner-darwin-amd64 /usr/local/bin/anka-gitlab-runner
chmod +x /usr/local/bin/anka-gitlab-runner
```

Confirm that the binary now exists:

```shell
❯ anka-gitlab-runner status                
Runtime platform                                    arch=amd64 os=darwin pid=19465 revision=1fb7c012 version=12.10.0/1.0-rc1
anka-gitlab-runner: Service is not running.
```

### Installing

> When installing, ensure you're in the directory you want to be used for the working directory (and that the user as access/permissions to it)

```shell
# macOS
❯ anka-gitlab-runner install  
Runtime platform arch=amd64 os=darwin pid=39598 revision=c505cd17 version=12.10.0/1.0-rc1

# Linux
$ sudo anka-gitlab-runner install
Runtime platform                                    arch=amd64 os=linux pid=147005 revision=66bf0b24 version=12.10.0/1.0-rc1
FATAL: Please specify user that will run gitlab-runner service 
$ sudo anka-gitlab-runner install -u testUser
Runtime platform                                    arch=amd64 os=linux pid=147054 revision=66bf0b24 version=12.10.0/1.0-rc1
success
```

> On Linux, it will run as root and execute jobs as the user specified by the install command. This means that some of the job functions like cache and artifacts will need to execute /usr/local/bin/gitlab-runner command, therefore the user under which jobs are run, needs to have access to the executable.

On macOS, the `install` command will place an `anka-gitlab-runner.plist` file into `~/Library/LaunchAgents/` (`/Library/LaunchAgents` if root user) so that the runner is available on restarts.

The `.plist` will contain `--working-directory` (which is set to the current location when running `anka-gitlab-runner install`) and `--config` (set to `macOS: $HOME/.gitlab-runner/anka-config.toml` / `linux: /etc/gitlab-runner/anka-config.toml`).

> You can uninstall with `anka-gitlab-runner uninstall`