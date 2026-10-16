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
