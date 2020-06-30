
> Perform the following steps on the machines you intend to be Nodes.
> "Nodes" are the host machines you want to run the Anka VMs. You can use any Apple hardware for this.

You can find the various Anka Build packages on the [Anka Build download page](https://veertu.com/download-anka-build/). You can use the guided installer by launching the .pkg, or install from your terminal.

#### Download the latest Anka PKG
```shell
curl -L -o Anka.pkg https://veertu.com/downloads/ankabuild-latest
```

#### Install the Anka PKG
```shell
sudo installer -pkg Anka.pkg -tgt /
```
Or, if you need nested virtualization to run Docker inside of the VM:

> You can find Nested Virtualization inside of the Custom Install section of the guided installer. You can get there using the `Customize` button on the **Installation Type** stage.

> We currently only support Docker within Nested Virtualization

```shell
sudo installer -applyChoiceChangesXML nanka.xml -pkg Ankaxx.pkg -target /
```

> You'll need modify your Anka VM Template and Tags with `anka modify name set nested 1`

#### Verify the installation
```shell 
anka version
```
- The output should be similar to `Anka Build Basic version 2.1.2 (build 112)`.

#### Activate your Anka license
```shell
sudo anka license activate <key>
```