
You can find the various Anka installation packages on the [veertu.com website](https://veertu.com/download-anka-build/).

## Download and install the latest Anka Virtualization package

```shell
curl -L -o AnkaVirtualization.pkg https://veertu.com/downloads/anka-virtualization
sudo installer -pkg AnkaVirtualization.pkg -tgt /
```

## Verify the installation

```shell 
anka version
```

The output should be similar to `Anka... version 2.X.X (XXXX)`.

---

> If you need Nested Virtualization to run Docker inside of the VM, you can find it by clicking the `Customize` button on the **Installation Type** stage of the interactive installer or adding the following to the installer command in your terminal: `sudo installer -applyChoiceChangesXML nanka.xml -pkg AnkaVirtualization.pkg -target /`
>
> When you generate your VM Template, be sure to enable it with: `anka modify name set nested 1`
>
> Only Docker is supported at this time.