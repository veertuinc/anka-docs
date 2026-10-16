---
date: 2026-10-09T01:00:00-00:00
title: "Anka Virtualization 3.10.0"
---

## USB Passthrough for Silicon

USB passthrough for Silicon is now generally available.

![USB Passthrough for Silicon]({{< siteurl >}}images/whats-new/anka-3.10.0/usb-passthrough-for-silicon.png)

## `anka create` reuses a macOS build without needing an IPSW

The first `anka create` for a macOS build still downloads the IPSW and installs it. After the install, Anka stores a tag named `.base_<ProductBuild>` on that VM and deletes the downloaded IPSW from the image library.

The next `anka create` for the same ProductBuild clones that tag. It does not download the IPSW again. The clone copies the tag onto the new VM, so later creates can still reuse the disk data after you delete the first VM.

```bash
❯ anka create vm1 latest
❯ anka create vm2 latest
```

`vm1` installs from the IPSW. `vm2` clones the stored tag.

`latest` and an explicit version or IPSW share a tag only when they resolve to the same ProductBuild.


{{< hint info >}}
Set `ANKA_CREATE_FORCE=1` to skip the stored tag and install from the IPSW again:

```bash
❯ ANKA_CREATE_FORCE=1 anka create vm3 latest
```
{{< /hint >}}

## Supporting OCI Layers

Anka is now comptible with OCI layers. They fall under Anka Template Tags, so we'll call them OCI Tags for the rest of this guide.

As expected with OCI layers with other tools, an OCI tag from one VM can be pulled onto any other VM. The other VM can have a different name, a different macOS version, or live on another host. Keep the vanilla macOS disk as a block tag in the Anka Registry, and pull that tag when a host needs the foundation. Store each tool as its own OCI tag in an OCI registry, and pull those tags when the VM needs them.

{{< hint warning >}}
**Store OCI tags in an OCI registry.** Use an `oci://` URL or an `https://` URL whose path contains `/v2/`. The Anka Registry does not store these types of tags.
{{< /hint >}}

{{< hint info >}}
OCI layers compression can be controlled with the `ANKA_COMPRESSION_LEVEL` environment variable. Setting it to `1` turns on gzip and sets zlib’s level to 1 (fastest / lightest compression).
{{< /hint >}}

OCI tags and Anka Registry tags behave differently. Here are some important differences to be aware of:

- OCI tags are stored only in an OCI registry.
- Pulling an OCI tag that is already on the VM shows `already up-to-date`. It does not remove contents from the VM. A pull of a new Anka Registry tag will remove the other OCI tags you pulled and revert back to its original state. Whereas with Anka Registry tags, on pull, you'll get the exact state of the VM when the tag was created, always, even if you made changes but haven't pushed a new tag.
- OCI tags stack on a VM. They do not remove contents from the VM (unless you [whiteout]({{< relref "whats-new/anka-3.10.0/index.md#remove-a-file-with-a-whiteout" >}}) a file in the archive you pull). Pull an OCI tag onto a VM that already has an Anka Registry tag on it, and you can revert to it whenever you mess up the OCI tags and want a clean slate.
- Anka Registry tags and OCI tags can be combined only if you first pull the Anka Registry tag.

### Pull OCI tags

Pull more than one OCI tag onto the same VM.

{{< hint info >}}
Later pulls can come from other OCI registries.
{{< /hint >}}

Let's use Homebrew as an example. Homebrew publishes bottles as OCI archives. This means we can pull `cmake` onto an Anka VM **without installing Homebrew, adding a symlink, or changing `PATH`**.

By default, a bottle stores its files under a version directory such as `cmake/4.3.4`. Anka's `--transform` option rewrites that prefix to `usr/local`, so `cmake` is exactly where you want it for the default PATH to see it.

```bash
❯ anka pull 27.0.1 oci://ghcr.io/homebrew/core/cmake:4.3.4 --transform 's|cmake/4.3.4|usr/local' --dry-run
usr/local/
usr/local/.brew/
usr/local/.brew/cmake.rb
usr/local/LICENSE.rst
usr/local/README.rst
usr/local/bin/
usr/local/bin/ccmake
usr/local/bin/cmake
usr/local/bin/cpack
usr/local/bin/ctest
. . .
anka pull 27.0.1 oci://ghcr.io/homebrew/core/cmake:4.3.4 --transform 's|cmake/4.3.4|usr/local'
100% [||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||]          
9eec878a-7f16-492f-97e9-88e7247e43a5: pulled 19.20MiB for tag cmake--4.3.4
```

You'll see the files in the VM's `/usr/local` directory.

{{< imgwithlink src="images/whats-new/anka-3.10.0/cmake-in-vm.png" width="500" >}}

You'll also see the tag locally under `anka list` and `anka show` commands.

```bash
❯ anka show 27.0.1 tag
+--------------+--------------------+----------------------+---------+----------+
| tag          | name               | creation_date        | state   | size     |
+--------------+--------------------+----------------------+---------+----------+
| vanilla      | 27.0.1             | Oct 12 11:48:18 2026 | stopped | 29.45GiB |
+--------------+--------------------+----------------------+---------+----------+
| cmake--4.3.4 | 27.0.1             | Oct 2 15:19:08 2026  | stopped | 256MiB   |
+--------------+--------------------+----------------------+---------+----------+
```

If I wanted, I could put a default in /usr/local, then install a different version of cmake into /usr/local, but with the versions folder.

```bash
❯ anka pull 27.0.1 oci://ghcr.io/homebrew/core/cmake:4.4.4 --transform 's|cmake|usr/local/cmake' --dry-run
usr/local/cmake/4.4.4
usr/local/cmake/4.4.4/.brew/
usr/local/cmake/4.4.4/.brew/cmake.rb
usr/local/cmake/4.4.4/LICENSE.rst
usr/local/cmake/4.4.4/README.rst
usr/local/cmake/4.4.4/bin/
usr/local/cmake/4.4.4/bin/ccmake
usr/local/cmake/4.4.4/bin/cmake
usr/local/cmake/4.4.4/bin/cpack
usr/local/cmake/4.4.4/bin/ctest
❯ anka pull 27.0.1 oci://ghcr.io/homebrew/core/cmake:4.4.4 --transform 's|cmake|usr/local/cmake'
100% [||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||]          
9eec878a-7f16-492f-97e9-88e7247e43a5: pulled 19.24MiB for tag cmake--4.4.4
❯ anka show 27.0.1 tag
+--------------+--------------------+----------------------+---------+--------+
| tag          | name               | creation_date        | state   | size   |
+--------------+--------------------+----------------------+---------+--------+
| cmake--4.3.4 | 27.0.1             | Jun 19 00:32:17 2026 | stopped | 256MiB |
+--------------+--------------------+----------------------+---------+--------+
| cmake--4.4.4 | 27.0.1             | Oct 2 15:19:08 2026  | stopped | 231MiB |
+--------------+--------------------+----------------------+---------+--------+
```

{{< hint warning >}}
Deletion of a tag from the VM (like `anka delete 27.0.1 --tag cmake--4.4.4`) does NOT remove the data from the VM. To revert to the state prior to the OCI tag pull, you need to have an Anka Registry tag to revert to with a pull.
{{< /hint >}}

### Push OCI tags

Machines that push to an OCI registry or an Anka Registry need the right `push_mode`. `push_mode` `1` pushes the full tag, including its base tag. `push_mode` `2` pushes only the changes since the base tag. The example below uses `1` for the macOS tag and `2` for the tool tags.

#### `push_mode` `1`

This is the default in Anka 3.10.x. The push includes the base tag. It can push to the Anka Registry and to an OCI registry.

{{< hint info >}}
Remember, both OCI and the Anka Registry try to optimize disk usage by reusing existing disk images in the registry, no matter if they are OCI tags or Anka Registry tags.
{{< /hint >}}

Example: `27.0.1-arm64:vanilla` is already on a host. You `anka pull` cmake onto that VM, then push the result as `27.0.1-arm64:cmake`.

On an Anka Registry or an OCI registry, `cmake` is a new tag whose base is the `vanilla` tag. The push reuses the `vanilla` tag the registry already has, so that tag stays unchanged. The registry stores the `cmake` tag on top of it.

#### `push_mode` `2`

The push stores the changes since the base tag. The base tag stays out of the manifest, so you can apply the tool tag onto another VM. Use this mode for an OCI registry.

With `push_mode` `2`, that same push stores the file changes. The macOS base disk stays out of the manifest.

#### Example

Create a new VM. `anka create` installs macOS only. Leave `push_mode` at `1` for this push. `1` is the default. This push uploads the macOS disk as a block tag to the Anka Registry in `ANKA_REGISTRY`. You can also push to an OCI registry with this mode.

```bash
❯ anka create 27.0.1 27.0.1
❯ anka push 27.0.1 -t vanilla http://anka.registry:8089
```

`http://anka.registry:8089` is the Anka Registry. An OCI registry URL has a `/v2/` path. This push creates template `27.0.1` and tag `vanilla` on the Anka Registry. A new `27.0.1` VM reports about `29.82GiB` uploaded to tag `vanilla`. Pull this tag when a host needs the macOS disk.

Set `push_mode` to `2` before you push the tool tags.

```bash
❯ anka config push_mode 2
```

{{< hint info >}}
`ANKA_PUSH_MODE` sets the mode for one command. Use it when a change to `anka config` is a problem. Example: `ANKA_PUSH_MODE=2 anka push 27.0.1 -t xcode-27.1 oci://registry.example.com/v2/`
{{< /hint >}}

Install one tool, stop the VM, and push the next tag to the OCI registry. Leave the repository name off the URL. Anka uses the VM name.

```bash
❯ anka start 27.0.1
# install Xcode in the VM, then stop it
❯ anka stop 27.0.1
❯ anka push 27.0.1 -t xcode-27.1 oci://registry.example.com/v2/
```

The `xcode-27.1` push uploads only the file changes since `vanilla`.

Install CMake and stop the VM.

```bash
❯ anka start 27.0.1
# install CMake in the VM, then stop it
❯ anka stop 27.0.1
```

The tag chain on this VM is now the `vanilla` tag, then the `xcode-27.1` tag, then the CMake changes. Push `cmake` with one of the two commands below. Note that `--thin` works with `push_mode` `1` and with `push_mode` `2`.

##### Without `--thin`

Without `--thin`, the push writes every diff after the base disk into the `cmake` tag (which is the current behavior customers are used to). The first layer is the `vanilla` tag's own diff. On this VM that diff has no file changes, so the layer is a 1024-byte tar with no files. The next layer is the Xcode diff. The last layer is the CMake diff. The pull reads only the `cmake` tag. It does not fetch the `xcode-27.1` tag. The Xcode files are in the `cmake` tag.

Pull `cmake` onto a second VM that is a clone of `vanilla`. Anka copies the Xcode files and the CMake files onto that VM. A second pull of the same tag prints `already up-to-date`.

```bash
❯ anka push 27.0.1 -t cmake oci://registry.example.com/v2/
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:cmake
```

##### With `--thin`

`anka pull --thin` applies only the last layer in that tag. On this `cmake` tag, that layer is the CMake diff. The Xcode diff in the same tag is skipped. On a clone of `vanilla`, this pull copies the CMake files and leaves the Xcode files out.

```bash
❯ anka pull 15.6.1 --thin oci://registry.example.com/v2/27.0.1:cmake
```

With `--thin` on the push, the `cmake` tag stores only the CMake diff. The Xcode diff stays in the `xcode-27.1` tag. A pull of `cmake` applies the CMake layer alone. Pull `xcode-27.1` first when the VM also needs Xcode. Pull the `vanilla` tag from the Anka Registry first when the host does not have that tag.

```bash
❯ anka push 27.0.1 -t cmake --thin oci://registry.example.com/v2/
❯ anka pull 27.0.1 -t vanilla http://anka.registry:8089
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:xcode-27.1
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:cmake
```

Anka stores each pulled OCI tag locally as `27.0.1--xcode-27.1` and `27.0.1--cmake`.

A later push of that VM with `push_mode` `2`, after one `--thin` pull, stores two file layers. One layer is the changes already on the VM. The other layer is the thin tag. The base tag, such as `vanilla`, stays out.

```bash
❯ anka push 15.6.1 -t cmake-on-15 oci://registry.example.com/v2/
❯ anka -j registry show oci://registry.example.com/v2/15.6.1:cmake-on-15 --manifest
```

To publish those tools onto a template that is already in the registry, put that template name in the URL. This is the same as `--remote-vm` / `-v`. Use `-f` when the local tag already exists.

```bash
❯ anka push builder -f -t xcode-brew https://registry.example.com/v2/shared
```

{{< hint info >}}
With `push_mode` `2`, if `shared` is a different VM than `builder` and it already has tags, this adds the new software onto `shared`'s newest tag. To add onto one tag by name:

```bash
❯ anka push builder -t xcode-brew https://registry.example.com/v2/shared:tools
```

A `push_mode` `2` push creates `tools` when that tag is absent. The registry tag name is `tools`. The local tag name stays `xcode-brew`.
{{< /hint >}}

A pull that is only a URL creates the VM for you. Point `default_vmid` at a VM you already have. A UUID is the stable choice. Add `:<tag>` to start from that tag.

```bash
❯ anka config default_vmid <vm-uuid>
❯ anka pull oci://ghcr.io/homebrew/core/cmake:4.3.4 --transform 's|cmake/4.3.4|usr/local'
```

Anka creates a VM named `cmake` from that VM and pulls the bottle into `usr/local`. You do not need to install Homebrew, add a symlink, or change `PATH`. A job that only has the registry URL can use this form.

### Export and import an OCI tag

`anka export` and `anka import` move a tag between hosts when you do not use a registry. Export has no `--thin` flag. With `push_mode` `1`, export of the first tag writes the VM. That tag has no parent tag. Import of that archive creates a VM that boots and keeps the tag. With `push_mode` `2`, export writes the diff for the tag you name. The `vanilla` diff is empty, so that export does not include the macOS disk. `xcode-27.1` exports the Xcode files. `cmake` exports the CMake files. `anka export -c` does not add the base tag while `push_mode` is `2`. Import with `-v` applies that diff onto a VM that already exists.

```bash
❯ anka export 27.0.1 xcode-27.1.tar -t xcode-27.1
❯ anka import xcode-27.1.tar -v 15.6.1
```

`15.6.1` must already exist. Import applies that tag's layer onto it. Import the tags in the order you created them.

[Exporting and importing VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms/exporting-and-importing-vms.md" >}}) describes the rest of the command, including the license requirement.

### Add an archive to an existing OCI tag

You can append a second archive to an OCI tag that already exists. The archive contains the files and folders you want to add. Anka adds that archive as the next layer on the same tag.

```bash
❯ anka push my.tar -t tag http://registry.example.com/v2/repo
```

`-t` is required. Without `-t`, Anka exits before the upload. An OCI tag is an ordered set of layers. The tag must already exist. A later pull applies those layers in manifest order. Anka pulls the first archive, then the archive you appended. A zero-length `.wh.` file in a later archive removes that file from the VM.

#### Remove a file with a whiteout

A later archive can also remove a file from an earlier layer. Add a zero-length whiteout file in the same directory. Name it `.wh.` plus the file name. Anka deletes that file from the final image.

`usr/local/bin/.wh.cmake` removes `usr/local/bin/cmake` from the earlier layer. A directory entry named `.wh.` plus the directory name removes that directory.

`anka push` writes these whiteout files when a file was deleted in the VM. You can also put them in an archive yourself. Use that archive to patch an existing OCI tag. You do not rebuild the earlier layers.

Show the manifest to see that order. `--manifest` requires `-j`.

```bash
❯ anka -j registry show http://registry.example.com/v2/repo:tag --manifest
```

### Preparing a build VM

You can prepare a build VM on one producer host, or on each runner with an Ankafile.

{{< hint info >}}
`anka create` and `from darwin:` use the stored `.base_<ProductBuild>` tag from above. The first create on a host downloads the IPSW. The next create for that ProductBuild on the same host clones that tag. A runner that only pulls the finished template receives macOS inside that template.
{{< /hint >}}

#### On a producer host

The order of the OCI tags matters on the producer host. Create the VM, then pull each OCI tag in that order. Push the finished VM as a template.

```bash
❯ anka create build_vm 27.0.1
❯ anka pull build_vm http://registry.example.com/v2/xcode:27.1
❯ anka pull build_vm https://ghcr.io/homebrew/core/cmake:4.4.4
❯ anka push build_vm -t latest
```

`anka push` uses the registry in `ANKA_REGISTRY`. To use a different registry, add the registry URL:

```bash
❯ anka push build_vm -t latest http://registry.example.com/v2
```

A runner host does not have the OCI tag order. It does not know which OCI tags the producer used. It pulls the template and starts it. That registry can be the Anka Registry. It can also be an OCI registry.

```bash
❯ anka pull build_vm
❯ anka start build_vm
```

#### With an Ankafile

An Ankafile stores the same steps, including the OCI tag order. Each runner can create the VM from that file. You can then remove the producer host from the fleet.

Pass the Ankafile as the second argument of `anka create`. The second argument must not be an `.ipsw` path. An `.ipsw` path is an installer, and Anka creates the VM from that installer.

```bash
❯ anka create build_vm ./Ankafile
❯ anka create build_vm .
❯ anka create build_vm - < ./Ankafile
```

`anka create <name> <file>` reads that file. `anka create <name> <directory>` reads `<directory>/Ankafile`. `anka create <name> -` reads the Ankafile from standard input. The `< ./Ankafile` redirect is that input.

```text
FROM darwin:27.0.1
FROM http://registry.example.com/v2/xcode:27.1
FROM https://ghcr.io/homebrew/core/cmake:4.4.4
```

`FROM macos:<version>` or `FROM darwin:<version>` creates a VM. The text after the colon is the macOS version. With no colon, the version is `latest`. The VM name is the name on the `anka create` command, or the name after `AS`.

`FROM <vm>` clones that VM. `FROM <vm> AS <name>` sets the new VM name.

`FROM <registry-url>` pulls that URL. A URL contains `/`. The first `FROM` creates the VM from the pull. A later `FROM` pulls onto that same VM.

`ARG name=value` sets a variable. `$name` and `${name}` expand from an `ARG` or from the host environment. These variables are already set: `TARGETPLATFORM`, `TARGETOS`, `TARGETARCH`, `TARGETVARIANT`, `BUILDPLATFORM`, `BUILDOS`, `BUILDARCH`, and `BUILDVARIANT`.

`LABEL <key> <value>` runs `anka modify <vm> label`. `ENV name=value` sets an environment variable for the `anka` commands that the build runs. `EXPOSE <port>` runs `anka modify <vm> port`. `MOUNT <path>` runs `anka modify <vm> mount`.

Anka stores `WORKDIR`. No instruction uses that path. Any other instruction, including `RUN` and `COPY`, stops the build. Anka reports that the command is unsupported.

When the build finishes, Anka stops the VM and prints its id. When the build fails, Anka deletes the VM it created.

You can also run the Ankafile on a producer host and push the template. Runners then pull that template, as in the section above.

```bash
❯ anka create build_vm ./Ankafile
❯ anka push build_vm -t latest
```