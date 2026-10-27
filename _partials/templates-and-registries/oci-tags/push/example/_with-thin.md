`--thin` on the push stores only the last layer. The `cmake` tag is then a thin tag that holds the CMake files. The Xcode files stay in the `xcode-27.1` tag. Pull `xcode-27.1` first when the VM also needs Xcode. Pull the `vanilla` tag first when the host does not have that tag.

```bash
❯ anka push --thin 27.0.1 -t cmake oci://registry.example.com/v2/
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:xcode-27.1
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:cmake
```

Anka stores each pulled thin tag locally as `27.0.1--xcode-27.1` and `27.0.1--cmake`.

`--thin` on a pull applies only the last layer in that tag. Use it when a tag still holds more than one layer, and you want only the last one:

```bash
❯ anka pull --thin 15.6.1 oci://registry.example.com/v2/27.0.1:cmake
```
