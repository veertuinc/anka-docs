Pull more than one thin tag onto the same VM.

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
Deletion of a tag from the VM (like `anka delete 27.0.1 --tag cmake--4.4.4`) does NOT remove the data from the VM. To go back to the state before the thin tag pull, pull a full tag again. The full tag can come from the Anka Registry or from an OCI registry.
{{< /hint >}}

### Pull a GGUF model from Hugging Face

Starting in version 3.10, Anka can pull GGUF models that [Hugging Face](https://huggingface.co) and [Ollama](https://ollama.ai) publish as OCI artifacts. The flow is the same as a Homebrew bottle: give `anka pull` the VM name and the `oci://` URL. Anka applies standard tar layers and layers with media type `application/vnd.ollama.image`.

```bash
❯ anka pull 27.0.1 oci://hf.co/MaziyarPanahi/Llama-3.2-1B-Instruct-GGUF:Q4_K_M
```

The text after the colon is the quantization, such as `Q4_K_M`. Anka stores the model under the guest user's Hugging Face Hub cache (by default `~/.cache/huggingface/hub`). A later job can load the weights from that cache. The job does not need a large download from the internet at run time.

Set `ANKA_MODEL_CACHE` to a path relative to the guest home directory when you need a different cache root.
