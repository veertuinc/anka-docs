You can append a second archive to a thin tag that already exists. The archive contains the files and folders you want to add. Anka adds that archive as the next layer on the same tag.

```bash
❯ anka push my.tar -t tag http://registry.example.com/v2/repo
```

`-t` is required. Without `-t`, Anka exits before the upload. A thin tag is an ordered set of file layers. The tag must already exist. A later pull applies those layers in manifest order. Anka pulls the first archive, then the archive you appended. A zero-length `.wh.` file in a later archive removes that file from the VM.
