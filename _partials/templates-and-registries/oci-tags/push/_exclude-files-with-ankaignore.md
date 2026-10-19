When you push or export a VM with `push_mode` `2`, Anka does not include the paths in the `ankaignore` file. The default file is `/Applications/Anka.app/Contents/Resources/ankaignore`. It excludes caches, logs, temporary files, and Spotlight data.

To exclude more paths, copy the default file, add your paths, and set the `ankaignore` config key to your copy:

```bash
❯ cp /Applications/Anka.app/Contents/Resources/ankaignore ~/ankaignore
❯ echo 'Users/*/Downloads' >> ~/ankaignore
❯ anka config ankaignore ~/ankaignore
```

To use a different file for one command only, set `ANKA_ANKAIGNORE`:

```bash
❯ ANKA_ANKAIGNORE=~/xcode.ankaignore ANKA_PUSH_MODE=2 anka push xcode-builder -t xcode-26.1 oci://registry.example.com/v2/xcode
```

Anka also uses a `.ankaignore` file in the current directory. Put one in each tool's build folder, then run `anka push` from that folder:

```bash
❯ cd ~/tools/xcode
❯ cp /Applications/Anka.app/Contents/Resources/ankaignore .ankaignore
❯ echo 'Users/*/Downloads' >> .ankaignore
❯ ANKA_PUSH_MODE=2 anka push xcode-builder -t xcode-26.1 oci://registry.example.com/v2/xcode
```

Anka uses only one file. It uses the first file that it finds, in this order:

1. `.ankaignore` in the current directory.
2. The file in `ANKA_ANKAIGNORE`.
3. The file in the `ankaignore` config key. If you do not change it, this is the default file.

Your file replaces the default file, so keep the default lines. The rules are like `.dockerignore`:

- Each path is relative to the root of the VM disk, with no leading `/`. A path without a `/` matches that name in any folder.
- `*` matches one folder level. `**` matches any number of levels.
- When you exclude a folder, Anka also excludes all of its contents.
- A line that starts with `!` keeps a path that a line above it excludes.
- A line that starts with `#` is a comment.
