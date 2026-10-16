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
