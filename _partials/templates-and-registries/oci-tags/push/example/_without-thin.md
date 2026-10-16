Without `--thin`, the push writes every diff after the base disk into the `cmake` tag (which is the current behavior customers are used to). The first layer is the `vanilla` tag's own diff. On this VM that diff has no file changes, so the layer is a 1024-byte tar with no files. The next layer is the Xcode diff. The last layer is the CMake diff. The pull reads only the `cmake` tag. It does not fetch the `xcode-27.1` tag. The Xcode files are in the `cmake` tag.

Pull `cmake` onto a second VM that is a clone of `vanilla`. Anka copies the Xcode files and the CMake files onto that VM. A second pull of the same tag prints `already up-to-date`.

```bash
❯ anka push 27.0.1 -t cmake oci://registry.example.com/v2/
❯ anka pull 15.6.1 oci://registry.example.com/v2/27.0.1:cmake
```
