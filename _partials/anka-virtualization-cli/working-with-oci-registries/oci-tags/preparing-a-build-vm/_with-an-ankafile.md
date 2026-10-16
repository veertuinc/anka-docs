---
---

{{< hint warning >}}
Later versions will support `RUN` and `COPY`. Later versions will also add instructions for Anka Click and for more VM configuration. `NETWORK` will run `anka modify <vm> network`. `RAM` will run `anka modify <vm> ram`. `CPU` will run `anka modify <vm> cpu`.
{{< /hint >}}

An Ankafile stores the same steps, including the OCI tag order. Each runner can create the VM from that file. You can then remove the producer host from the fleet.

The result of an Ankafile is always one VM. An Ankafile does not make a multistage build.

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

Anka stores `WORKDIR`. No instruction uses that path. `RUN`, `COPY`, and any other instruction stop the build. Anka reports that the command is unsupported.

When the build finishes, Anka stops the VM and prints its id. When the build fails, Anka deletes the VM it created.

You can also run the Ankafile on a producer host and push the template. Runners then pull that template, as in the section above.

```bash
❯ anka create build_vm ./Ankafile
❯ anka push build_vm -t latest
```
