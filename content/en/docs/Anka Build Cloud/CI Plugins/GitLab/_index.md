---
date: 2019-07-03T22:24:47-05:00
title: "Using GitLab and Anka Build Cloud"
linkTitle: "GitLab"
weight: 8
description: >
  Instructions on how to use GitLab with Anka Build Cloud
---

If you are using GitLab, Veertu provides and maintains the [Anka GitLab Runner](https://github.com/veertuinc/gitlab-runner). The runner connects GitLab to the Anka Cloud Controller to perform VM Instance creation and command execution through SSH into the VM Instance.
 
## VM Template & Tag Requirements

> The below list are the absolute necessities needed to execute commands in a VM through your CI and the GitLab Runner. You may have to include other dependencies depending on your needs.

1. In the VM:
    - Install `git`
    - Make sure remote login is enabled (`System Preferences > Sharing`).
2. On the host, enable [port forwarding]({{< relref "docs/Anka CLI/command-reference.md#example---add-port-forwarding" >}}) for your VM Template using the Anka CLI. _We recommend not specifying `--host-port`._
3. `sudo anka suspend {VM Template name}`
4. `sudo anka registry push {VM Template name} {Tag name}`

## Preparing the Anka GitLab Runner

> These steps are based on GitLab 12.9.4 (9d231b25b49); Your version/UI may differ slightly.

> You can run the Anka GitLab Runner on any machine with network access to the Anka Cloud Controller, Anka Nodes, and your GitLab.

### Downloading

You can find our releases on the [Anka GitLab Runner GitHub repo](https://github.com/veertuinc/gitlab-runner/releases).

You can then curl down the version you need for your OS and architecture:

> We'll be using an amd64 mac for our guide. However, the commands also work for Linux with slight alterations.

```shell
curl -L https://github.com/veertuinc/gitlab-runner/releases/download/1.0-rc1/anka-gitlab-runner-darwin-amd64.tar.gz
tar -xzvf anka-gitlab-runner-darwin-amd64.tar.gz
cp -f anka-gitlab-runner-darwin-amd64 /usr/local/bin/anka-gitlab-runner
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

## GitLab Setup

Once the runner is installed, you can set it up as:

1. A **Shared Runner**
2. A repo's **Specific Runner**
3. Both

However, all of those require a Registration Token. Here are the GitLab UI locations where you can find that Token:

1. **Shared Runner**: Admin Area > Overview > Runners (/admin/runners)
2. **Specific Runner**: Under your repo > Settings > CI / CD > Click on Expand next to Runners

### Registration

With the token, you can now register using `anka-gitlab-runner register` (guided / prompts) or with `--non-interactive`:

```shell
anka-gitlab-runner register --non-interactive \
--url "http://anka.gitlab:8084/" \
--registration-token nHKqG3sYV4B5roRK1ZhW \
--ssh-user anka \
--ssh-password admin \
--name "localhost shared runner" \
--anka-controller-address "http://my.controller.url" \
--anka-template-uuid d09f2a1a-e621-463d-8dfd-8ce9ba9f4160 \
--anka-tag base:port-forward-22:brew-git:gitlab \
--executor anka \
--tag-list "localhost-shared,localhost,iOS"
```

You can find all of the options available with `--help`:

```
❯ anka-gitlab-runner-darwin-amd64 register --help 

Runtime platform                                    arch=amd64 os=darwin pid=45449 revision=b2f4ad1b version=12.10.0/1.0-rc1
NAME:
   anka-gitlab-runner-darwin-amd64 register - register a new runner

USAGE:
   anka-gitlab-runner-darwin-amd64 register [command options] [arguments...]

OPTIONS:
   -c value, --config value              Config file (default: "/Users/nathanpierce/.gitlab-runner/anka-config.toml") [$CONFIG_FILE]
   --template-config value               Path to the configuration template file [$TEMPLATE_CONFIG_FILE]
   --tag-list value                      Tag list [$RUNNER_TAG_LIST]
   -n, --non-interactive                 Run registration unattended [$REGISTER_NON_INTERACTIVE]
   --leave-runner                        Don't remove runner if registration fails [$REGISTER_LEAVE_RUNNER]
   -r value, --registration-token value  Runner's registration token [$REGISTRATION_TOKEN]
   --run-untagged                        Register to run untagged builds; defaults to 'true' when 'tag-list' is empty [$REGISTER_RUN_UNTAGGED]
   --locked                              Lock Runner for current project, defaults to 'true' [$REGISTER_LOCKED]
   --access-level value                  Set access_level of the runner to not_protected or ref_protected; defaults to not_protected [$REGISTER_ACCESS_LEVEL]
   --maximum-timeout value               What is the maximum timeout (in seconds) that will be set for job when using this Runner (default: "0") [$REGISTER_MAXIMUM_TIMEOUT]
   --paused                              Set Runner to be paused, defaults to 'false' [$REGISTER_PAUSED]
   --name value, --description value     Runner name (default: "Veertu.local") [$RUNNER_NAME]
   --limit value                         Maximum number of builds processed by this runner (default: "0") [$RUNNER_LIMIT]
   --output-limit value                  Maximum build trace size in kilobytes (default: "0") [$RUNNER_OUTPUT_LIMIT]
   --request-concurrency value           Maximum concurrency for job requests (default: "0") [$RUNNER_REQUEST_CONCURRENCY]
   -u value, --url value                 Runner URL [$CI_SERVER_URL]
   -t value, --token value               Runner token [$CI_SERVER_TOKEN]
   --tls-ca-file value                   File containing the certificates to verify the peer when using HTTPS [$CI_SERVER_TLS_CA_FILE]
   --tls-cert-file value                 File containing certificate for TLS client auth when using HTTPS [$CI_SERVER_TLS_CERT_FILE]
   --tls-key-file value                  File containing private key for TLS client auth when using HTTPS [$CI_SERVER_TLS_KEY_FILE]
   --executor value                      Select executor (anka or ssh) [$RUNNER_EXECUTOR]
   --builds-dir value                    Directory where builds are stored [$RUNNER_BUILDS_DIR]
   --cache-dir value                     Directory where build cache is stored [$RUNNER_CACHE_DIR]
   --clone-url value                     Overwrite the default URL used to clone or fetch the git ref [$CLONE_URL]
   --env value                           Custom environment variables injected to build environment [$RUNNER_ENV]
   --pre-clone-script value              Runner-specific command script executed before code is pulled [$RUNNER_PRE_CLONE_SCRIPT]
   --pre-build-script value              Runner-specific command script executed after code is pulled, just before build executes [$RUNNER_PRE_BUILD_SCRIPT]
   --post-build-script value             Runner-specific command script executed after code is pulled and just after build executes [$RUNNER_POST_BUILD_SCRIPT]
   --debug-trace-disabled                When set to true Runner will disable the possibility of using the CI_DEBUG_TRACE feature [$RUNNER_DEBUG_TRACE_DISABLED]
   --shell value                         Select bash, cmd or powershell [$RUNNER_SHELL]
   --custom_build_dir-enabled            Enable job specific build directories [$CUSTOM_BUILD_DIR_ENABLED]
   --cache-type value                    Select caching method [$CACHE_TYPE]
   --cache-path value                    Name of the path to prepend to the cache URL [$CACHE_PATH]
   --cache-shared                        Enable cache sharing between runners. [$CACHE_SHARED]
   --cache-s3-server-address value       A host:port to the used S3-compatible server [$CACHE_S3_SERVER_ADDRESS]
   --cache-s3-access-key value           S3 Access Key [$CACHE_S3_ACCESS_KEY]
   --cache-s3-secret-key value           S3 Secret Key [$CACHE_S3_SECRET_KEY]
   --cache-s3-bucket-name value          Name of the bucket where cache will be stored [$CACHE_S3_BUCKET_NAME]
   --cache-s3-bucket-location value      Name of S3 region [$CACHE_S3_BUCKET_LOCATION]
   --cache-s3-insecure                   Use insecure mode (without https) [$CACHE_S3_INSECURE]
   --cache-gcs-access-id value           ID of GCP Service Account used to access the storage [$CACHE_GCS_ACCESS_ID]
   --cache-gcs-private-key value         Private key used to sign GCS requests [$CACHE_GCS_PRIVATE_KEY]
   --cache-gcs-credentials-file value    File with GCP credentials, containing AccessID and PrivateKey [$GOOGLE_APPLICATION_CREDENTIALS]
   --cache-gcs-bucket-name value         Name of the bucket where cache will be stored [$CACHE_GCS_BUCKET_NAME]
   --preparation-retries value           Set the amount of preparation retries for a job (default: "0") [$PREPARATION_RETRIES]
   --ssh-user value                      User name [$SSH_USER]
   --ssh-password value                  User password [$SSH_PASSWORD]
   --ssh-host value                      Remote host [$SSH_HOST]
   --ssh-port value                      Remote host port [$SSH_PORT]
   --ssh-identity-file value             Identity file to be used [$SSH_IDENTITY_FILE]
   --anka-controller-address value       Anka Cloud Controller address (example: http://anka-controller.mydomain.net[:8090]) [$CONTROLLER_ADDRESS]
   --anka-template-uuid value            Specify the VM Template UUID [$TEMPLATE_UUID]
   --anka-tag value                      Specify the Tag to use [$TAG]
   --anka-node-id value                  Specify the Node ID to run the job (you can find this in your Controller's Nodes page) [$NODE_ID]
   --anka-priority value                 Set the job priority [$PRIORITY]
   --anka-group-id value                 Run on a specific Controller Node group [$GROUP_ID]
   --anka-root-ca-path value             Specify the path to your Controller's Root CA certificate [$ROOT_CA_PATH]
   --anka-cert-path value                Specify the path to the GitLab Certificate (used for connecting to the Controller) (requires you also specify the key) [$CERT_PATH]
   --anka-key-path value                 Specify the path to your GitLab Certificate Key (used for connecting to the Controller) [$KEY_PATH]
   --anka-skip-tls-verification          Skip TLS Verification when connecting to your Controller [$SKIP_TLS_VERIFICATION]
   --anka-keep-alive-on-error            Keep the VM alive for debugging job failures [$KEEP_ALIVE_ON_ERROR]
```

> To unregister, you can use `anka-gitlab-runner unregister -n "localhost shared runner"`

You should now see the runner in your GitLab:

![gitlab runner attached](/images/gitlab-runner-attached.png)

You can now start the anka-gitlab-runner: 

```shell
❯ anka-gitlab-runner start          
Runtime platform                                    arch=amd64 os=darwin pid=36048 revision=66bf0b24 version=12.10.0/1.0-rc1
executed                                           
```

Ensure that GitLab shows **just now** under the **Last contact** column.

## Using our Docker images 

[Docker Hub - i386 Repo](https://hub.docker.com/repository/docker/veertu/anka-gitlab-runner-i386)

[Docker Hub - amd64 Repo](https://hub.docker.com/repository/docker/veertu/anka-gitlab-runner-i386)

When executing `docker run`, any arguments included are used as options for `anka-gitlab-runner register --non-interactive`:

```shell
❯ docker run -ti --rm veertu/anka-gitlab-runner-amd64 --help
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Runtime platform                                    arch=amd64 os=linux pid=693 revision=6c1a9836 version=12.10.0/1.0-rc1
NAME:
   anka-gitlab-runner register - register a new runner

USAGE:
   anka-gitlab-runner register [command options] [arguments...]

OPTIONS:
   -c value, --config value 
. . .
```

> We run `update-ca-certificates` each time you start the container. You can add a volume to mount in your certificates if needed.

You use the same non-interactive arguments that we mentioned in [the Registration section]({{< relref "docs/Anka Build Cloud/CI Plugins/GitLab/_index.md#registration" >}}) when executing the binary (but without `--non-interactive`):

```shell
❯ docker run -ti --rm veertu/anka-gitlab-runner-amd64 --url "http://anka.gitlab:8084/" --registration-token nHKqG3sYV4B5roRK1ZhW --ssh-user anka --ssh-password admin --name "localhost shared runner" --anka-controller-address "https://anka.controller:8080/" --anka-template-uuid d09f2a1a-e621-463d-8dfd-8ce9ba9f4160 --anka-tag base:port-forward-22:brew-git:gitlab --executor anka --anka-root-ca-path /Users/nathanpierce/anka-ca-crt.pem --anka-cert-path /Users/nathanpierce/anka-gitlab-crt.pem --anka-key-path /Users/nathanpierce/anka-gitlab-key.pem --clone-url "http://anka.gitlab:8084" --tag-list "localhost-shared,localhost,iOS"
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Runtime platform                                    arch=amd64 os=linux pid=693 revision=7b532a90 version=12.10.0/1.0-rc1
Running in system-mode.                            
                                                   
Registering runner... succeeded                     runner=nHKqG3sY
Updated:  /etc/gitlab-runner/anka-config.toml      
Feel free to start anka-gitlab-runner, but if it's running already the config should be automatically reloaded! 
Runtime platform                                    arch=amd64 os=linux pid=705 revision=7b532a90 version=12.10.0/1.0-rc1
Starting multi-runner from /etc/gitlab-runner/anka-config.toml...  builds=0
Running in system-mode.                            
                                                   
Configuration loaded                                builds=0
listen_address not defined, metrics & debug endpoints disabled  builds=0
[session_server].listen_address not defined, session endpoints disabled  builds=0
```

When you stop or exit the container, it will automatically unregister it from your GitLab.

---

## Answers to Common Questions

- If you see "Missing anka-gitlab-runner. Uploading artifacts is disabled" at the end of jobs that have archiving enabled, [you need to install the anka-gitlab-runner in your VM Tag](https://stackoverflow.com/questions/45567587/uploading-artifacts-is-disabled).
- We've defaulted preparation retries to 2, but you can modify this using:

    ```
    --preparation-retries value           Set the amount of preparation retries for a job (default: "0") [$PREPARATION_RETRIES]
    ```
- We've stripped out everything but the anka and ssh executors. Fortunately, the `anka-gitlab-runner` is independent and doesn't conflict with the regular `gitlab-runner` if they're running on the same machine. 
  - The options that aren't associated with the stripped out executors you should still be able to use (cache-s3, maximum-timeout, etc).
- We use slightly modified versions of the original `gitlab-runner` tests (they didn't pass when we forked 12.10.0).
- If your controller goes down, the runner will retry the HTTP calls for several minutes until finally throwing an error.
