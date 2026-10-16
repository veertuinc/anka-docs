`anka pull --thin` applies only the last layer in that tag. On this `cmake` tag, that layer is the CMake diff. The Xcode diff in the same tag is skipped. On a clone of `vanilla`, this pull copies the CMake files and leaves the Xcode files out.

```bash
❯ anka pull 15.6.1 --thin oci://registry.example.com/v2/27.0.1:cmake
```

With `--thin` on the push, the `cmake` tag stores only the CMake diff. The Xcode diff stays in the `xcode-27.1` tag. A pull of `cmake` applies the CMake layer alone. Pull `xcode-27.1` first when the VM also needs Xcode. Pull the `vanilla` tag first when the host does not have that tag, as shown in the first step.

```bash
❯ anka push 27.0.1 -t cmake --thin oci://registry.example.com/v2/
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:xcode-27.1
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:cmake
```

Anka stores each pulled OCI tag locally as `27.0.1--xcode-27.1` and `27.0.1--cmake`.
