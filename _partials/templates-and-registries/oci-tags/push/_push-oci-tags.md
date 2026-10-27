Use `--thin` when you push a tool as a thin tag. `--thin` pushes only the last layer, so you do not change `push_mode`. Leave `push_mode` at `1` (the default). Then a push without `--thin` still makes a full tag that includes the macOS disk.

`push_mode` `2` is another way to make a thin tag. It stores only the file changes since the base tag. It needs a config change or `ANKA_PUSH_MODE=2` on the command. A push to the Anka Registry does not use `--thin` or `push_mode`. The Anka Registry always stores a full tag.
