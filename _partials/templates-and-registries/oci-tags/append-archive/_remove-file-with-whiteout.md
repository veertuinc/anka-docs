A later archive can also remove a file from an earlier layer. Add a zero-length whiteout file in the same directory. Name it `.wh.` plus the file name. Anka deletes that file from the final image.

`usr/local/bin/.wh.cmake` removes `usr/local/bin/cmake` from the earlier layer. A directory entry named `.wh.` plus the directory name removes that directory.

`anka push` writes these whiteout files when a file was deleted in the VM. You can also put them in an archive yourself. Use that archive to patch an existing thin tag. You do not rebuild the earlier layers.

Show the manifest to see that order. `--manifest` requires `-j`.

```bash
❯ anka -j registry show http://registry.example.com/v2/repo:tag --manifest
```
